---
layout: post
title: Booting Home Assistant OS on TrueNAS/bhyve
---

I love using Home Assistant to control all of my smart home devices and create
cool automations. But I've found that running it on Raspberry Pis or even the
[Home Assistant Blue](https://www.home-assistant.io/blue/) to be somewhat unreliable.
In the case of Raspberry Pis, they boot from SD cards which are notoriously unreliable.
In the case of the Home Assistant Blue, it boots from an internal eMMC card,
which is better but still not perfect. I also had hardware issues with the ODroid
N2+ in the Home Assistant Blue, my USB ports died which rendered it useless to me
since many of my smart home devices are Z-Wave, which requires a Z-Wave USB stick
to act as the base station.

Instead, I wanted to run Home Assistant OS (the fully managed Linux distro
built by Home Assistant) on my NAS, which is an old Dell R510 server with dual
socket Xeons, 24gb of RAM, and 8 hot swap hard drive bays. I currently run TrueNAS,
which only supports the bhyve hypervisor. However, the Home Assistant OS team only
provide VirtualBox, QEMU and VMWare images. However, it is fairly easy to convert
the QEMU `.qcow2` to a raw image file, and boot it under bhyve.

# Step 1: Create a Zvol for the VM disk
Under Storage/Pools, expand the ZFS pool you want to create the Zvol under.
Click the 3 dot options menu, and select add `Add ZVol`. Add a name and set the size
to 40gb.

# Step 2 (optional):
If you need USB support in the VM for Z-Wave or Zigbee, you'll need to pass through
a PCIe USB interface. Depending on your hardware, you may need to add a USB card.
I used [this card](https://www.amazon.com/dp/B072LS4JH7) and it works well.

You will need to set a few FreeBSD tunables in order for the USB interface to be
able to be passed through to the VM.
[Follow these steps](https://wiki.freebsd.org/bhyve/pci_passthru).
After setting those tunables, you'll need to reboot TrueNAS.

# Step 3: Create and configure the VM
In the `Virtual Machines` page, click the `Add` button to add a VM. Choose Linux
as the guest OS, enter a name, cloose system clock as UTC, and click `Next`.
Add CPU and memory resources as needed, and click `Next`. Choose `Use existing disk image`,
and select the Zvol created in step 1. No other settings need to be changed, so
continue pressing `Next` until the VM is created.

Next, go back to the Virtal Machine page and expand the info for the VM. Select
Devices, then Add, select type PCI Passthru Device, select the PCI Passthru device
and save.

# Step 4: Download the Home Assistant OS image and convert it to raw
Download the [KVM image](https://www.home-assistant.io/installation/linux) from
Home Assistant. Extract the image (for this step you'll need Linux):

```
$ unxz haos_ova-X.X.qcow2.xz
```

Next, convert the image to a raw disk image:

```
$ sudo apt install qemu-utils
$ qemu-img convert haos_ova-X.X.qcow2 haos.raw
```

Next, you'll need to move the image to your TrueNAS server:

```
$ scp haos.raw user@truenas.local:</path/to/haos.raw>
```

Now you'll need to write the image over top of the Zvol created in step 1.

SSH into TrueNAS (or use the shell from the UI), and run the following as root.
Replace the necessary paths, and be careful since we're using
[dd(1)](https://www.freebsd.org/cgi/man.cgi?dd(1))
which is a fairly dangerous command. You can very easily destroy your data, and you
should be backing up your data regularly both onsite and offsite.

When running anything dangerous like dd, it's a good idea to type a `#` first, and
then type out your command. Once you've quadruple checked the paths and syntax,
use the arrow keys to go back and remove the `#`.

```
# dd if=</path/to/haos.raw> of=</dev/zvol/your-pool-name/your-zvol-name>
```

# Step 5: Start the VM

At this point the VM should be ready to start, and you should be done! If you didn't
already, you probably want to set the VM to start on boot.
