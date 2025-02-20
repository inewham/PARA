# Install Klipper Touch

After the installation of the Klipper firmware the control unit on the Ender-3 V3 SE stops working, not sure if there is a solution for this, but I opted to remove it.  

I installed a 3.5 inch touch screen that attached to the GPIO pins.  
Below are the directions that I  followed to get it working correctly.

# Install and configure touch screen

```
./kiauh/kiauh.sh
```
- Select **_1_** to Install.
- Select **_7_** for Touch Screen GUI.
Follow the wizard opting for yes, to most of the questions, I opted not to setup a cron job.
- Enter **_B_** to return to the main menu.
- Enter **_Q_** to quit the application.

```
sudo nano /boot/firmware/config.txt
```
Amending the **_dtover=_** to **_dtoverlay=piscreen,drm_**.
```
sudo nano /boot/firmware/cmdline.txt
```
Add **_fbcon=map:11_** to the end of the line of text.
```
sudo reboot
```
Once the device has rebooted enter the following to obtain the device name.
```
DISPLAY=:0 xinput
```
Enter the following command to amend the touch overlay (ensure you test).
```
DISPLAY=:0 xinput set-prop "ADS7846 Touchscreen" 'Coordinate Transformation Matrix' -1 0 1 0 1 0 0 0 1
```
The above command is not persistent, and will not survive a reboot, edit the following file to address this.
```
sudo nano /etc/udev/rules.d/51-touchscreen.rules
ACTION=="add", ATTRS{name}=="ADS7846 Touchscreen", ENV{LIBINPUT_CALIBRATION_MATRIX}="-1 0 1 0 1 0 0 0 1"
```
 

# Reference

- https://www.amazon.co.uk/dp/B0DL9ZM13C?ref=ppx_yo2ov_dt_b_fed_asin_title
- https://klipperscreen.readthedocs.io/en/latest/Hardware/GPIO_35/

