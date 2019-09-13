#  SRAMP  #

This is a rewrite of the original Marlin firmware that comes with ROBO3D's R1+  3D printers.

The  objectives

Create a small and simple firmware for basic Mendel/Cartesian printers build on top of RAMPS 1.4 (1.6) and Arduino ATMEGA 2560.  

Optimized the code for the RAMPS/ATMEGA2560 and simple assumptions of the common printers features and modifications.

Make modofications and hacks easier!!!!!


## Boards ##

This firmware is exclusive for (RAMPS 1.4)[https://reprap.org/wiki/RAMPS_1.4]  boards and similar revisions including 1.6.  As the RAMPS board is build for the Arduino MEGA we are able to make a lot of clean up and optimizasion for this hardware combination.

## Motion and Steppers ##

This code uses the original motion and stepper control code with some extensive cleanup. The motion and stepper control code is from grbl. 

The firmware supports basix X, Y, Z (optional double z steppers). With up-to two extruders. Work on dual carriage/extruders is half-baked since I don't have a printer to test with. 

Endstops are hardwired to be at the origin of the axis with a negative direction. Z endstop is at bottom of axis. Enstop switches use pullup resistors.

## Thermosistors ##

Code is build around common EPCOS 100K Thermistors for the Hot bed and extruder [thermosistors](https://reprap.org/wiki/Thermistor).



CFG_Z_HOME_DIR is always -1 going down.

A new g-code parser was b

The whole project was cleanup and wrote to be compiled under Arduino IDE and  

Code uses stock arduino libraries for

I called SRAMP for "Small RAMPS" Firmware.
