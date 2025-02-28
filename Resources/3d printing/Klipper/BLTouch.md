# Complete Guide to Setting Up BL-Touch in Klipper

Ensure the following is listed in the printer.cfg file.
```
[bltouch]
sensor_pin: ^PC14 #These pins are specific to Ender-3 V3 SE mainboard
control_pin: PC13
x_offset: -24.0   # x offset for the BL touch to the nozzle
y_offset: -16.0   # y offset for the BL touch to the nozzle
z_offset: 2.085
samples: 2        # Number of samples taken at each probe point
speed: 20
pin_move_time: 0.4
#stow_on_each_sample: False
#probe_with_touch_mode: True
```
```
[safe_z_home]
home_xy_position: 134,123  # This should be the middle of the build plate.
speed: 50
z_hop: 10                  # How much the nozzle will move after homing.
z_hop_speed: 5
```
Within the printer.cfg file the **endstop_pin:** key should be changed to the following.  
**endstop_pin: probe:z_virtual_endstop**  
Then comment out **position_endstop:** as this will be used within the BLtouch section.

- Save and restart the firmware.

To confirm the BLtouch is functioning correctly issue the following command in the console.
```
bltouch_debug command=touch_mode
bltouch_debug command=pin_down
query_probe
```
- While pushing the pin up, run the query code again, this should return a **triggered** state.

## Configure X and Y Offset

- In the console, extend the probe pin.
- Place a PostIT note with a dot drawn on it under the probe. The probe should be directly over the dot.

Note the x and y coordinates (Ending Coordinates)
Now move the nozzle directly over the dot, and record the x and y coordinates (Starting Coordinates).

Now, using the **Ending Coordinates** subtract the **Starting Coordinates** this will give you the x and y offsets.

## Configure Z Offset

- Within the printer.cfg file, under the **stepper_z** section, ensure you have a **position_min:** key, with a value of negative six, if an error is received, you can increase the value.
- Ensure the printer is homed, then in the console enter **probe_calibrate**.
- Using the provide value buttons, perform the paper test on the nozzle (good tension, but can still move it).
- Once completed, select **ACCEPT** (or type **accept** into the console) and click **SAVE AND RESTART** (or type **save_config** into the console), the new setting will be saved at the bottom of the config file.

## Finding the MESH_MIN and MESH_MAX values

- Within the printer.cfg, ensure there is a **bed_mesh** section, if one is not present, add the below.
```
[bed_mesh]
speed: 50  #speed moved between probing points
horizontal_move_z: 5  #distance the nozzle risers after each probe
mesh_min: 16, 14    # mesh_min and mesh_max create a bondary box the probe will stay in
mesh_max: 208,211
probe_count: 5,5  #number of points along the x and y axis
mesh_pps: 3,3  #point calculated between probing points
fade_start: 1  #fade_start and fade_end are the measurements used to decrease the effectiveness of the bed mesh
fade_end: 10
fade_target: 0
algorithm: bicubic
```
- Home the print head.
- In the X coordinate box enter 0 (Zero) and press **Enter**.
- In the Y coordinate box enter 0 (Zero) and press **Enter** (note the probe is not over the bed).
- Using the probe control arrows, move the probe so it is roughly 10mm onto the print bed on both X and Y axis.
- Note the X and Y coordinates, and add the X and Y offset taken earlier, this will give the starting coordinates for the bed mesh, this becomes the **mesh_min** value.
- Within printer.cfg, under the stepper_x and stepper_y sections, find the position_max value.
- Enter these values into the X and Y coordinates control fields.
- Using stepper_x and stepper_y values, add the x and y offset, this becomes the **mesh_max** value.

# Reference
- https://www.youtube.com/watch?v=5vmjBXvY6BA&t=632s
- https://www.klipper3d.org/G-Codes.html?h=bltouch#bltouch_debug

10:32
