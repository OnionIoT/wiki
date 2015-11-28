# Connect to the Omega via Serial Terminal

[[_TOC_]]

## OSX

**Step 1**: Download and install the Silicon Labs CP2102 driver from [[https://www.silabs.com/Support%20Documents/Software/Mac_OSX_VCP_Driver.zip]].

**Step 2**: Run `ls /dev/tty.*` to see if the USB-to-Serial device can be detected. If the driver is successfully installed, you should be able to see a device with a name similar to `/dev/tty.SLAB_USBtoUART`.

![Check if serial device exists](//i.imgur.com/FLn2p35h.jpg "Check if serial device exists")

**Step 3**: Run `screen /dev/tty.SLAB_USBtoUART 115200` to connect to the Omega’s serial terminal using screen.

Press `ENTER` to activate the command prompt.

![Log in through serial terminal](//i.imgur.com/cGANJefh.jpg "Log in through serial terminal")

## Windows

**Step 1**: Download and install the Silicon Labs CP2102 driver from [[https://www.silabs.com/Support%20Documents/Software/CP210x_VCP_Windows.zip]].

**Step 2**: Run Computer Management (Start > Run… > Enter “Computer Management” and press ```ENTER```), look for Silicon Labs CP210x USB to UART Bridge under Ports (COM & LPT), and take note of the COM number in bracket.

![Computer Management](//i.imgur.com/0fFBiNNh.jpg "Computer Management")

**Step 3**: Download and install Putty from [[http://the.earth.li/~sgtatham/putty/latest/x86/putty-0.65-installer.exe]].

**Step 4**: Open up PuTTY, select Serial for Connection type, enter the COM number noted down in Step 2 as Serial line, and enter ```115200``` for the speed.

![Configure PuTTY](//i.imgur.com/jnREOQth.jpg "Configure PuTTY")

**Step 5**: Click on the Open button to connect to the Omega via the serial terminal.

Once connected, press `ENTER` to activate the command prompt.

![Log in through serial terminal](//i.imgur.com/d6INMZkh.jpg "Log in through serial terminal")

## Linux

**Step 1**: Download and install the Silicon Labs CP2102 driver source files.

For Linux kernel **3.x.x and higher**: [[https://www.silabs.com/Support%20Documents/Software/Linux_3.x.x_VCP_Driver_Source.zip]].
For Linux kernel **2.6.x**: [[https://www.silabs.com/Support%20Documents/Software/Linux_3.x.x_VCP_Driver_Source.zip]].

**Step 2**: Build and install the driver.

*For Ubuntu/Debian*:

Unzip the archive.

`cd` into the unzipped directory.

Compile the driver with `make`.

```
$ sudo cp cp210x.ko to /lib/modules/<kernel-version>/kernel/drivers/usb/serial/<kernel-version>
$ sudo insmod /lib/modules/<kernel-version>/kernel/drivers/usb/serial/usbserial.ko
$ sudo insmod cp210x.ko
$ sudo chmod 666 /dev/ttyUSB0
```

*For RedHat/CentOS*:

```
$ sudo yum update kernel* //need to update the kernel first otherwise your header won't match
$ sudo yum install kernel-devel kernel-headers //get the devel and header packages
$ sudo reboot //your build link should be fixed after your system come back
```

Unzip the archive.

`cd` into the unzipped directory.

Compile the driver with `make`.

```
$ sudo cp cp210x.ko to /lib/modules/<kernel-version>/kernel/drivers/usb/serial
$ sudo insmod /lib/modules/<kernel-version>/kernel/drivers/usb/serial/usbserial.ko
$ sudo insmod cp210x.ko
$ sudo chmod 666 /dev/ttyUSB0
```


**Step 3**: Run `ls /dev/ttyUSB*` to see if the USB-to-Serial device can be detected. If the driver is successfully installed, you should be able to see a device with a name similar to `/dev/ttyUSB0`.

![Check if serial device exists](//i.imgur.com/p1OwSE6h.png "Check if serial device exists")

**Step 4**: Run `screen /dev/tty.SLAB_USBtoUART 115200` to connect to the Omega’s serial terminal using screen.

Press `ENTER` to activate the command prompt.

![Log in through serial terminal](//i.imgur.com/sENEIX8h.png "Log in through serial terminal")
