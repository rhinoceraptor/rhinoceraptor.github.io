---
layout: post
title: Arduino Temperature Sensor
---

I previously worked on an enclosure for the Computer Club server rack, and I thought it would be a good idea to put together a standalone temperature sensor system. Preferably it would interface over the internet so that IRC bots and other programs could talk to it.

![Server rack cabinet](https://farm9.staticflickr.com/8612/16524942312_81b622a8aa_z_d.jpg)

Here is the project, using an Arduino Uno for testing.

![The project using an Arduino Uno](https://farm8.staticflickr.com/7339/16525954455_dae4872608_z_d.jpg)

The project will use four DHT-11 temperature sensors, an ATMega 328 with an Arduino bootloader (for ease of programming), and an ENC28J60 ethernet module. The microcontroller will take a temperature reading every few seconds, and when a connection comes in, it will send back JSON for easy parsing, in the following format:

```
{
  "sensor one": {
    "humidity": "34",
    "celcius": "25",
    "fahrenheit": "77"
  },
  ...
  "sensor four": {
    "humidity": "34",
    "celcius": "24",
    "fahrenheit": "75"
  }
}
```
![The project, fully breadboarded](https://farm8.staticflickr.com/7399/16338473020_17f4c32a53_z_d.jpg)

Here is the project built out on the breadboard.

The code, along with necessary libraries for this project is [available here](https://github.com/rhinoceraptor/arduino-server-sensor).

In a future post I will detail the wiring used in the project, as well as building the project on perfboard, or possibly designing and etching a custom PCB for the circuit.

