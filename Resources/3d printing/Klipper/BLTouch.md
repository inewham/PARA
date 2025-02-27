# Complete Guide to Setting Up BL-Touch in Klipper
[_TOC_]
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

# Reference

- https://www.klipper3d.org/G-Codes.html?h=bltouch#bltouch_debug

