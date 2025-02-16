# Install OctoScreen on OctoPi

Update and upgrade all system and software packages:
```
sudo apt update && sudo apt upgrade -y
```

Download and install the LCD screen driver:
```
git clone https://github.com/waveshare/LCD-show.git
cd LCD-show
sudo ./LCD35-show
sudo reboot
```
Configure screen resolution at boot:
```
sudo nano /boot/config.txt
hdmi_cvt 800 533 60 6 0 0 0
sudo reboot
```
Install relevant packages:
```
sudo apt install libgtk-3-0 xserver-xorg xinit x11-xserver-utils git build-essential xorg-dev xutils-dev x11proto-dri2-dev libltdl-dev libtool automake libdrm-dev -y
```
Download, compile and install  improved screen drivers:
```
git clone https://github.com/ssvb/xf86-video-fbturbo.git
cd xf86-video-fbturbo/
autoreconf -vi
./configure --prefix=/usr
make
sudo make install
sudo cp xorg.conf /etc/X11/
```
Download and install and configure OctoScreen package:
```
wget https://github.com/Z-Bolt/OctoScreen/releases/download/v2.7.4/octoscreen_2.7.4_armhf.deb
sudo dpkg -i octoscreen_2.7.4_armhf.deb
sudo apt-get install --fix-broken 
sudo nano /etc/octoscreen/config
sudo sed -i 's/OCTOSCREEN_RESOLUTION=.*/OCTOSCREEN_RESOLUTION=800x533/' /etc/octoscreen/config
sudo systemctl set-default graphical 
sudo reboot
```

## References:
https://github.com/z-bolt/OctoScreen/wiki/Installing-OctoScreen-with-a-3.5"-480x320-TFT-screen
https://www.youtube.com/watch?v=_MJLJz-pJAA&list=LL
https://www.amazon.co.uk/dp/B0DL9ZM13C?ref=ppx_yo2ov_dt_b_fed_asin_title
