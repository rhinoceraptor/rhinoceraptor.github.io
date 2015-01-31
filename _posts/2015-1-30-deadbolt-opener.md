---
layout: post
title: 3D Printed Servo Deadbolt Opener
---

### Overview

I wanted to design a part the would allow a common hobby servo to unlock a deadbolt. You could then use RFID or some other means of authentication to open a door.

I used the larger size available at Radio Shack.

![Common Hobby Servo](https://farm9.staticflickr.com/8593/15784047683_03cc051e13_z_d.jpg)

To start, I designed this mount in OpenSCAD, to be bolted or double side taped to a door, over the deadbolt:

![Servo mount](https://farm8.staticflickr.com/7334/16221026167_8ef630b0b6_z_d.jpg)

Then I needed a cup to fit over the deadbolt handle:

![Deadbolt Cup](https://farm9.staticflickr.com/8663/16380957196_cdd2093e7d_z_d.jpg)

On the other side, I used OpenSCAD code from Charles Rincheval for the servo spindle teeth, the code of which is available at [https://github.com/hugokernel/OpenSCAD_ServoArms](https://github.com/hugokernel/OpenSCAD_ServoArms).

![Servo Teeth](https://farm8.staticflickr.com/7284/16406012172_bdbc72a7a8_z_d.jpg)

Here is a test fit of the cup I made. It took two tries but it fits snugly.

![Printed deadbolt cup](https://farm8.staticflickr.com/7297/16219276838_5d179f1b31_z_d.jpg)

Here is the servo mounted to the base, the inset screw heads fit great. 

![Servo mount printed](https://farm8.staticflickr.com/7300/16220689909_16ea6b4149_z_d.jpg)

Here is the device, assembled and mounted, in the open position.

![Open position](https://farm8.staticflickr.com/7419/16220687689_758c59722b_z_d.jpg)

Here it is, in the closed position.

![Closed position](https://farm8.staticflickr.com/7297/16220687979_86d3ecf63a_z_d.jpg)

The OpenSCAD files used in this project are available at [https://github.com/rhinoceraptor/arduino-deadbolt-opener](https://github.com/rhinoceraptor/arduino-deadbolt-opener).

