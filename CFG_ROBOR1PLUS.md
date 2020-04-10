#  Frimware configuration for ROBO3D R1 Plus #

So details for configuring this firmware with a R1+ or modified it. 

> This firmware is already configured for an R1+. 


# Dimensions and Limits #



# Motion & Steps #

The firmware has a default Z offset of 1 to match the R1+ specs. 
```c
#define CFG_Z_PROBE_OFFSET_FROM_EXTRUDER 1
```
You can adjust this value by using a `M565`  in your [start g-code](http://wiki.mattercontrol.com/SETTINGS/Printer/Custom_G-Code/Start_G-Code).


R1+ uses Nema17 Stepper Motor with 810 GT2 belts. 

```c
// default steps per unit for RoBo 3D R1
#define DEFAULT_AXIS_STEPS_PER_UNIT   {80,80,800,723.38}  
```

Default feed rate for Homing:
```c
// set the homing speeds (mm/min) //robo
#define CFG_HOMING_FEEDRATE {50*60, 50*60, 10*60, 0}  // set the homing speeds (mm/min) //robo
```
```

