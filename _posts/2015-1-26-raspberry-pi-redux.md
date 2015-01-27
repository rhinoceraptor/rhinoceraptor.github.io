---
layout: post
title: Raspberry Pi Door Opener Redux
---

### New Room

When the Computer Club moved rooms, I wanted to improve the software that I had written for the door opener. It also turned out that the existing door opener was not able to open the new door. The old handle was very easy to turn, but the new one was more rigid, so I had to build an opener that had more torque.

![Side View of the new opener]({{ site.url }}/images/rpi-two/side-view.jpg)
I accomplished this using a NEMA 17 stepper motor, and gears printed on the Computer Club's RepRap 3D printer. The smaller gear is mounted using a captive nut to the stepper motor's axle, and it turns the larger gear. String wraps around the shaft of the larger gear, between it and the body of the lid.

![Top view of the new opener]({{ site.url }}/images/rpi-two/top-view.jpg)
The many small holes are the product of the [OpenSCAD script](http://www.thingiverse.com/thing:6544) I used to generate the gears. I needed a small hole to tie the string off with, and I'm not proficient enough to make a single small hole, so I had the script generate many holes so that they were small.

![Internal Wiring of the project]({{ site.url }}/images/rpi-two/wiring.jpg)
Here is the wiring for the project. I am using the [Easy Driver](http://www.schmalzhaus.com/EasyDriver/) stepper motor driver from SparkFun. All you need to do is connect your stepper motor's coils correctly, provide 12 volts, and then pulse the step and/or direction pins to control your stepper.

I am using a 5 volt relay, (triggered using an NPN transistor, one GPIO ping and the Raspberry Pi's 5 volt output) to turn off power to the Easy Driver board when the stepper is not in use. Stepper motors use a considerable amount of current while idle, and they can get pretty hot.

The power supply has a standard Molex power connector. I used JB weld to secure the male side of a Molex cable to the project box. The 5 volt connection is wired to a micro USB cable, and the 12 volts is wired through the relay as I described.


 

