# Spinrite in Virtualbox

# How to use Spinrite in a virtual machine within VirtualBox.

This guide assumes you have created a virtual machine and mounted a Spinrite ISO, which is the
primary boot device, and your host OS is Linux.

- Identify the drive you want to scan with Spinrite

```bash
***sudo fdisk –l***
```

Or

```bash
***dmesg***
```

- Unmount the drive.?
- Create a Raw disk:

```bash
s***udo VBoxManage internalcommands createrawvmdk -filename ~/rawdisk.vdi -rawdisk /dev/sdb***
```

- The ***'-rawdisk'*** path is obtained from the previous step.
- If working with SD cards the device maybe ***'/dev/mmcblk0'***.
- Launch VirtualBox with administrative privilages

```bash
***sudo virtualbox***
```

- Add the rawdisk to the virtual machine:
    - Select the ***'+'*** button, under ***'settings'***, ***'Storage'*** and ***'Controller: SATA'***.
    - Select ***'Choose existing disk'***.
    - Select ***'rawdisk.vdi'***, and click ***'Open'***.
- Start the virtual Machine.