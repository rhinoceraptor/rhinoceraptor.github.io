---
layout: post
title: How I built a keyboard by hand
---

![The finished product](https://farm9.staticflickr.com/8746/30166008906_e0ac5eb5b2_k_d.jpg)

I had spare mechanical keyboard switches and keycaps from other keyboard projects, and I wanted to see if it would be feasible to build a keyboard using only hand tools.

![Testing the layout](https://farm6.staticflickr.com/5195/30085878312_5dce857134_k_d.jpg)

I started with some aluminum sheet stock from the hardware store. It was perfectly sized to build a 5 column, 13 row grid keyboard, without any wasted space.

![Scribed aluminum sheet](https://farm6.staticflickr.com/5146/30115575711_dcf028bc00_k_d.jpg)

I started by laying out where the switch holes would be cut. The holes are 14 mm squares, with 5.5 mm in between. I used a blade to scribe the layout into the metal.

![Test fitting a switch](https://farm6.staticflickr.com/5260/30166053386_b6716ac935_k_d.jpg)

I drilled a hole for each switch, and filed them square.

![First two rows cut](https://farm6.staticflickr.com/5628/30115561731_eb20eaa807_k_d.jpg)

![Test fitting first two rows](https://farm6.staticflickr.com/5468/30115558701_c8f07f5f15_k_d.jpg)

![Completed keyboard plate](https://farm6.staticflickr.com/5008/30115556111_e159eb5fb8_k_d.jpg)

Once the top plate was done, I used another piece of the same aluminum stock for the bottom of the keyboard, with metal standoffs to complete the case.

![Keyboard case assembly](https://farm6.staticflickr.com/5130/30166046486_9d0175777c_k_d.jpg)

![Test fitting all switches](https://farm6.staticflickr.com/5594/29904585790_9586465493_k_d.jpg)

I painted the case black. This helps hide the misaligned keys, and makes it look more uniform.

![Painted case](https://farm6.staticflickr.com/5215/29571182703_ea05a1cfed_k_d.jpg)

![Test fitting switches and keycaps after painting](https://farm6.staticflickr.com/5128/30200419565_21edb9f4d7_k_d.jpg)

I used 1N4148 diodes to connect the rows together, and wired the columns together. I referred to the [Atreus keyboard hand-wiring guide](https://atreus.technomancy.us/assembly-hand-wired.pdf) to help with the wiring.

![Column and row wiring](https://farm8.staticflickr.com/7484/29571168153_4c94df32ef_k_d.jpg)

Each of the columns and rows are connected to a Teensy microcontroller. I had to take care to avoid the standoffs when wiring the keyboard.

![Complete wiring](https://farm6.staticflickr.com/5557/29571161073_047f4003c0_k_d.jpg)

I used a micro USB extension cable to complete the keyboard, and glued the end of it to the bottom of the case.

![USB extender glued to case](https://farm6.staticflickr.com/5553/29904544790_6f279a5252_k_d.jpg)

![Top view of keyboard](https://farm9.staticflickr.com/8624/29904541790_be33c79916_k_d.jpg)

To program the keyboard, I used the open source [QMK firmware](https://github.com/jackhumbert/qmk_firmware). I started with the [Atreus keyboard](https://atreus.technomancy.us) configuration, and edited the configuration header file to match the wiring of the rows and columns. I then edited the keymap header for the keyboard layout. The QMK firmware provides macros for function layers, and will automatically produce a standard shift layer.

Then, (after installing some build tools), I compiled the firmware and flashed it to the Teensy using the Teensy loader tool provided by PJRC.

