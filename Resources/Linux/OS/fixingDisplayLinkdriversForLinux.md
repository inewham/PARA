# Fixing DisplayLink (EVDI) drivers for Linux kernel 6.x

Kernel 6.x broke the support for the EVDI which is shipped with current release (5.6.1) of the [DisplayLink drivers for Ubuntu](https://www.synaptics.com/products/displaylink-graphics/downloads/ubuntu). This is really annoying as all of a ‘sudden’ my workstations which sport [Pop!_OS 22.04 LTS](https://pop.system76.com/) wouldn’t work with external monitors, connected using a DisplayLink device, anymore. This happened by an upgrade package overnight (around 25th of October 2022). It took me a little while to realize what the actual problem is.

## Updates

Updates, having the most recent on top, on the current status of the EVDI trouble.

- **Update 2023-MAR-10**: Kernel 6.2 broke the EVDI again: awaiting for [a fix to be implemented](https://github.com/DisplayLink/evdi/pull/401). Possible solution, suggested by Daniel C., is using [kernelstub](https://support.system76.com/articles/kernelstub/) to assign the 6.0.2 kernel to you system again; `sudo kernelstub -v -k /boot/vmlinuz-6.0.6-76060006-generic -i /boot/initrd.img-6.0.6-76060006-generic`
- **Update 2022-DEC-02**: Pop!_OS updated the kernel again, this time to version `6.0.6-76060006-generic`. Again no dice… re-applying the procedure, as noted below, fixed the issue again.

## Culprit

I noticed, during experimenting with the different 22.04 releases of Pop!_OS, the DisplayLink device wasn’t displaying to the external monitors anymore althought the other functionality (USB, Ethernet and audio) was working as expected. That real culprit has to do with the kernel being updated from 5.19.0-x to 6.0.2-x and the display part of the drive. After diggin around I discovered that the [EVDI](https://displaylink.github.io/evdi/) version isn’t working anymore after the kernel updates.

## Linux kernel 5.x - DisplayLink working as expected.

When re-installing the [Pop!_OS 22.04_14 ISO](https://iso.pop-os.org/22.04/amd64/intel/14/pop-os_22.04_amd64_intel_14.iso), sporting the `5.19.0-76051900-generic` kernel again; **No problem!**

## Linux kernel 6.x - DisplayLink, internal laptop display only!

As a new version, [Pop!_OS 22.04_15](https://iso.pop-os.org/22.04/amd64/intel/15/pop-os_22.04_amd64_intel_15.iso), running the `6.0.2-7606002-generic` kernel, was released; **no additional monitors!**

This made me vigurously looking what was going on. From the log sfiles, it became clear to me that the kernel was bugging the compatibility with the EVDI drivers.

So, I decided to look into that direction and stumbled upon [this question on GitHub](https://github.com/DisplayLink/evdi/issues/383), submitted by [BasSmeets](https://github.com/BasSmeets). This led me to the forum post by [Karly](https://www.displaylink.org/forum/member.php?u=23243) on the [DisplayLink forum](https://www.displaylink.org/forum/showpost.php?p=92453&postcount=3)

## Solution

The solution is to replace the shipped evdi.tar.gz file, containing the [EVDI driver](https://displaylink.github.io/evdi/), in the installed package with a newer version. This can be done by following a simple procedure.

As I am using the `displaylink-driver-5.6.1-59.184.run` file, I will use this as the base line version. In your case, you may need to adapt the version numbers accordingly.

## Download the latest driver

First create a folder to temporarily store the drivers in.

```
cd ~
mkdir displaylinkfix && cd displaylinkfix
```

At the moment the 5.6.1 is the latest driver from the [Synaptics download site](https://www.synaptics.com/products/displaylink-graphics/downloads/ubuntu).

The zip file can be downloaded using;

```
wget https://www.synaptics.com/sites/default/files/exe_files/2022-08/DisplayLink%20USB%20Graphics%20Software%20for%20Ubuntu5.6.1-EXE.zip
```

Unzipping the file is easy peasy

```
unzip DisplayLink\ USB\ Graphics\ Software\ for\ Ubuntu5.6.1-EXE.zip 
```

## Uninstall the current EVDI driver.

The current installed drivers need to be uninstalled, adding `uninstall` parameter to the `.run` file is needed.

```
sudo ./displaylink-driver-5.6.1-59.184.run uninstall
```

## Extracting the content.

Now the `.zip` file is unpacked the `.run` is present. The content of the `.run` archive file is what is needed, so unpack it.

```
./displaylink-driver-5.6.1-59.184.run --noexec --keep

cd ./displaylink-driver-5.6.1-59.184
```

## Update the EVDI source files

Replace the `evdi.tar.gz` file with the latest version from the DisplayLink GitHub repository.

```
curl -L https://github.com/DisplayLink/evdi/archive/refs/heads/devel.tar.gz -o evdi.tar.gz
```

## Script modification

The downloaded `evdi.tar.gz` archive contains an extra folder which the embedded `evdi.tar.gz` omits. So while extracing the file the path needs to altered by using the `--strip-components=1` parameter to the `tar` command.

Modify, using you favourite text editor, the `install_evdi()` function of the`displaylink-installer.sh` file, around line #39:

```
if ! tar xf "$TARGZ" -C "$EVDI"; then
```

and replace it with:

```
if ! tar xf "$TARGZ" -C "$EVDI" --strip-components=1; then
```

## Run the installer script as root

This installs and compiles the latest EVDI driver which works with the current 6.0.2 kernel. To make the drivers effective reboot your machine when prompted.

```
sudo ./displaylink-installer.sh
```

## Enjoy the multi-monitor setup again.

![https://code.berrydejager.com/assets/img/Fix-DisplayLink_drivers-linux-kernel-6-whoohoo.gif](https://code.berrydejager.com/assets/img/Fix-DisplayLink_drivers-linux-kernel-6-whoohoo.gif)

## Gratitude

Thanks the following people:

- [Karly](https://www.displaylink.org/forum/member.php?u=23243)
    - For pointing out a solution
- [BasSmeets](https://github.com/BasSmeets)
    - For creating an issue on GitHub
- [Wesly](https://github.com/wezzynl)
    - For thinking along with me.
