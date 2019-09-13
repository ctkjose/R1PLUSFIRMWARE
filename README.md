#  SRAMP  #

This is a rewrite of the original Marlin firmware that comes with ROBO3D's R1+  3D printers. This project was born from the frustuation of trying to modify the firmware.

## The  objectives ##

Create a small and simple firmware for basic Mendel/Cartesian printers build on top of RAMPS 1.4 (1.6) and Arduino ATMEGA 2560.  

Optimized the code for the RAMPS/ATMEGA2560 and simple assumptions of the common printers features and modifications.

Make modofications and hacks easier!!!!!


## Boards ##

This firmware is exclusive for [RAMPS 1.4](https://reprap.org/wiki/RAMPS_1.4)  boards and similar revisions including 1.6.  As the RAMPS board is build for the Arduino MEGA we are able to make a lot of clean up and optimizations for this hardware combination.

## Motion and Steppers ##

This code uses the original motion and stepper control code with some extensive cleanup. The motion and stepper control code is from grbl. 

The `planner.cpp` has a lot of cleanup and optimizations for our particular hardware setup. 

The firmware supports basix X, Y, Z (optional double z steppers). With up-to two extruders. Work on dual carriage/extruders is half-baked since I don't have a printer to test with. 

Endstops are hardwired to be at the origin of the axis with a negative direction. Z endstop is at bottom of axis. Enstop switches use pullup resistors.

## Bed Heater and Thermistors ##

Code is based on the marlin's `temperature.cpp`, it is mainly a cleanup and optimizations for our hardware.

Code is build around common EPCOS 100K Thermistors for the Hot bed and extruder [thermosistors](https://reprap.org/wiki/Thermistor).

## G-Code ##

This firmware includes a new g-code parser, that allows for parsing more complicated command syntax. The parser and gcode processing is completely new, the print job and processing is very modular and structured. 

G-CODE commands are implemented as function callbacks. We can register function pointers to a given G-CODE. Adding or modifying a G-CODE is easier and safer.

The print job is now represented by a `state` struct that allows code on different parts of the firmware to easily get info about of the state of the printer and a print-job and interact with the current job.

A print-job is represented by an object that allows us to control and interact with the printer. It has clear and specific places to poll and update time critical events during a print job and hardware interrupts. So now we have a place to call hooks to update an LCD, read buttons, or update something. In addition we added basic HOOKS so we can add new functionality at given events. 

## SD Card ##

SD Card uses Arduino's built-in library for SPI SD-Cards. 

GCODE [`M20`](https://reprap.org/wiki/G-code#M20:_List_SD_card) supports JSON with the `S2` parameter just like RepRapFirmware.

Writting to the SD Card is still not implemented.


