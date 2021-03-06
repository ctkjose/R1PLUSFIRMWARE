#  SRAMP  #

This is a rewrite of the original [Marlin firmware](https://github.com/Robo3D/R1PlusMarlin) that comes with ROBO3D's R1+  3D printers. This project was born from the frustuation of trying to modify the firmware.

As printers and technology becomes more advance so does these firmwares adding so much code not needed for basic mendel printers in particular older ones. 



## The  objectives ##

Create a small and simple firmware for basic Mendel/Cartesian printers build on top of RAMPS 1.4 (1.6) and Arduino ATMEGA 2560.  

Optimized the code for the RAMPS/ATMEGA2560 and simple assumptions of the common printers features and modifications.

Make modofications and hacks easier!!!!!

Optimize for size and performance under Arduino's ATMEGA 2560.

Keep compatibility with slicers and applications like MatterControl and Cura. 

Add modern printer control features as supported by RAMPS.

## Thanks and Copyrights ##

This project is based on original work from the [Marlin](http://marlinfw.org) [frimware](https://github.com/MarlinFirmware/Marlin) and projects like Sprinter and grbl.<br>
Copyright (C) 2016 MarlinFirmware<br>

Marlin is  based on a mashup between [Sprinter](https://github.com/kliment/Sprinter) and [grbl](https://github.com/simen/grbl/tree).<br>
Copyright (C) 2011 Camiel Gubbels / Erik van der Zalm<br>

Thanks to the guys at [RepRap](https://reprap.org/wiki/RepRap) and the community for all the valuable information that made this project possible.


## Building and uploading to your printer ##

This firmware was last tested with Arduino IDE 1.8.12.

In Arduino IDE, select your board from the Tools > Board menu.  You should see "Arduino mega or Mega 2560".

Make sure your printer is connected and not in use by a 3D printing software like MatterControl or Cura.

Select the serial USB port your board is connected to in the Tools > Port menu.

Open the `firmware.ino` in Arduino IDE.

Click the Verify button at the top of the window to test for configuration errors. 

Click Upload to flash the firmware to your board.

## Boards ##

This firmware is exclusive for [RAMPS 1.4](https://reprap.org/wiki/RAMPS_1.4)  boards and similar revisions including 1.6.  As the RAMPS board is build for the Arduino MEGA we are able to make a lot of clean up and optimizations for this hardware combination.



## Motion and Steppers ##

This code uses the original motion and stepper control code with some extensive cleanup. The motion and stepper control code is originally from [grbl](https://github.com/grbl/grbl). 

The `planner.cpp` has a lot of cleanup and optimizations for our particular hardware setup. 

The firmware supports basic X, Y, Z (optional double z steppers). With up-to two extruders. Work on dual carriage/extruders is half-baked since I don't have a printer to test with. 

Endstops are hardwired to be at the origin of the axis with a negative direction. Z endstop is at bottom of axis. Enstop switches use pullup resistors.

## Bed Heater and Thermistors ##

Code is based on the marlin's `temperature.cpp`, it is mainly a cleanup and optimizations for our hardware.

The code is build with two assumptions:

1. The printer has a hot bed.
2. EPCOS 100K (like) Thermistors are used for the hot bed and extruder [thermosistors](https://reprap.org/wiki/Thermistor).

## G-Code ##

This firmware includes a new g-code parser, that allows for parsing more complicated command syntax. The parser and gcode processing is completely new. Everything is structured and compartmentalized. 

A good G-CODE reference can be found [here](https://reprap.org/wiki/G-code), our flavor of G-CODE is more similar to RepRapFirmware and LinuxCNC.   

G-CODE commands are implemented as function callbacks. We can register function pointers to a given G-CODE. Adding or modifying a G-CODE is easier and safer.

The print job is now represented by a `state` with all the key data and information in one place. This makes it much easier to manipulate our job in different parts of the firmware and allows for better implementation of pause and resume.

A print-job is represented by an object that allows us to control and interact with the printer. It has clear and specific places to poll and update time critical events during a print job and hardware interrupts. So now we have a place to call hooks to update a LCD, read buttons, or update something. In addition we added basic HOOKS so we can add new functionality at given events. 

Code is structured to allow for further work on streaming g-code from other sources (ideas in my head includes streaming with wifi, and thru SPI from another arduino acting as a controller.)

The firmware has a standarized and consistant way to report errors to the g-code sender, even when CURA/MatterControl have no support for it (mainly because ther is no standard!).

(PS: Im also working on my own g-code sender and print scheduler).

## SD Card ##

SD Card uses Arduino's built-in library for SPI SD-Cards.

SD Card printing is working. This code uses RepRapFirmware flavor of GCODE and does not support Marlin's `!` and `#` syntax.

GCODE [`M20`](https://reprap.org/wiki/G-code#M20:_List_SD_card) supports JSON with the `S2` parameter just like RepRapFirmware.

Writting to the SD Card is still not implemented.


# Bed Leveling #

Auto bed leveling is eneble by default.  A  `G29` command will use a grid mode. By default 9 points will be sampled.

The rectangle to probe is defined in  `config.h` with the following constants:
```c
#define LEFT_PROBE_BED_POSITION 15  
#define RIGHT_PROBE_BED_POSITION 190  //width
#define BACK_PROBE_BED_POSITION 190	//height
#define FRONT_PROBE_BED_POSITION 15
```
This firmware uses a smaller rectangle for the auto bed leveling feature to support common 220x220 bed films/tapes and removable surfaces. 

Original values for the R1 are:
```c
#define LEFT_PROBE_BED_POSITION 15
#define RIGHT_PROBE_BED_POSITION 205
#define BACK_PROBE_BED_POSITION 230
#define FRONT_PROBE_BED_POSITION 20
```

## Cura ##

See [Robo3D/Slicing-Profiles](https://github.com/Robo3D/Slicing-Profiles).

| Setting | Value |
| -- | -- |
|  `machine_gcode_flavor`  |  `{ "default_value": "RepRap (Marlin/Sprinter)" }`  |

