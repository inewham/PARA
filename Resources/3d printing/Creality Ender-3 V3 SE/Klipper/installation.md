# Installation and configuration of Klipper on a Creality Ender-3 V3 SE

## Overview

For this installation I'll be using a Raspberry Pi 3B, this Pi also has a 3.5 inch touch screen

### Setup the Raspberry Pi

Using Raspberry Pi Imager, I've burned Raspberry Pi Lite 64bit OS to a 32GB MicroSD card, I've used the Imager function to preconfig the RPi OS to connect a my wireless network, create a user account and set the Locale.  
Once the OS has initially booted up, I'll be able to SSH into the device to complete the Klipper installation.

This document does not cover the end to end process of burning a Raspberry Pi OS to an SD card etc, as there are to many articles to count, but I've referenced the definitive source.

### Update and upgrade software packages

When you first install the Raspberry Pi (RPi) OS all the software packages will need updating, this can be quickly done by running the following command (then grab a brew).
```
sudo apt update && sudo apt upgrade
sudo reboot
```
This will update the software repositories, then upgrade all software, and reboot the device.

## Install Klipper Installation And Update Helper

I'll install Klipper Installation And Update Helper (KIAUH), which will be quicker in the long run, as we can use this to install additional features.  
First, we'll need to install the software package Git, by executing the following code.
```
sudo apt install git -y
```
Now, we'll clone the KIAUH repository by executing the following code.
```
git clone https://github.com/dw-0/kiauh.git
```
Now, we can begin the Klipper installation by executing the following.
```
./kiauh/kiauh.sh
```
If prompted to use KIAUH version 6, select yes.
- Select **_1_** for **_Install_**.
- Select **_1_** for  **_Klipper_**.
- Press **_Enter_** to accept the default instance number of **_1_**.
- Press **_n_** to select not to create an example printer.cfg file (grab another brew).
- Select **_y_** to add your user account to the **_tty_** group.
- Once the installation has finished, select **_2_**, to install **_Moonraker_**.
- Select **_y_** to create a config file.
- Select **_4_** to install **_Fluidd_**.
- Select **_y_** to download the config file.
- Press **_Enter_** to accept the default port.
- Enter **_B_** to go back to the parent menu.
- Enter **_Q_** to quit.
- Now you can browse the to Klipper web interface, via **_http://<IP_ADDRESS>_**
- Once you have access to the web UI, select the settings icon (cog) in the left hand navigation pane.
- Click on **_Authentication_**.
- Under the **_Authentication_** section, click on the **_ADD USER_** button.
- Enter a **_User_** name and **_Password_** and click **_ADD_**.

## Download printer.cfg

Enter the follow to download a printer.cfg file for the Ender-3 V3 SE.
```
cd ~/printer-data/config
touch marco.cfg
wget https://raw.githubusercontent.com/bootuz-dinamon/ender3-v3-se-full-klipper/refs/heads/main/printer.cfg
cd ~
```

## Create Firmware for printer

Now we need to create the firmware so Klipper can communicate with the mainboard, execute the following.
```
./kiauh/kiauh.sh
```
- Select **_4_** for **_Advanced_**.
- Select **_1_** for **_Build_**.
- Ensure **_Micro-contoller Arcitecture_** is highlighted, and press **_Enter_**.
- Ensure **_STMicroelectronics STM32_** is highlighted, and press **_Enter_**.
- Ensure **_Processor model_** is highlighted, and press **_Enter_**.
- Ensure **_STM32F103_** is highlighted, and press **_Enter_**.
- Ensure **_Bootloader offset_** is highlighted, and press **_Enter_**.
- Ensure **_28KiB bootloader_** is highlighted , and press **_Enter_**.
- Ensure **_Communication interface_** is highlighted, and press **_Enter_**.
- Ensure **_Serial (on USART1 PB10/PA9)_** is highlighted, and press **_Enter_**.
- Press Q and then Y.

The firmware will now start to compile.
- Press **_y_** to save the firmware configuration.
- Enter **_Ender3V3SE_** and press **_Enter_**.
- Press **_y_** to confirm the save location.
- Press **_B_** and the **_Q_** to exit KIAUH.
- Move the new bin file to printer-data/config folder by executing the following.
```
mv klipper/out/klipper.bin printer_data/config
```
- You will need to download **_klipper.bin_** from the RPi, to your local machine, if you are using Windows, you'll need to use WinSCP, if you are using Linux, execute the following.
```
cd ~
scp <USERNAME>@<IP_ADDRESS>:/home/<USERNAME>/printer_data/config/klipper.bin ./
```
- Copy **_klipper.bin_** to an SD card.
- Ensure the printer is turned off, and insert the SD card into the reader.
- Power on the printer.
- From the klipper web UI, select **_Configuration_**.
- Click on **_FIRMWARE RESTART_**.




# References

- https://www.raspberrypi.com/documentation/computers/getting-started.html#installing-the-operating-system
- https://github.com/dw-0/kiauh
- https://github.com/bootuz-dinamon/ender3-v3-se-full-klipper/tree/main?tab=readme-ov-file
- https://github.com/Klipper3d/klipper
