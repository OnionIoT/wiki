# Get Started with the Onion Omega

[[_TOC_]]

## Preparing the Hardware

**Step 1**: Unpack the Omega and Dock from the boxes.

![Omega + Dock](//i.imgur.com/tKs4wRWh.jpg "Omega + Dock")

**Step 2**: Connect the Omega to the Dock.

![Omega plugged into Dock](//i.imgur.com/rek12Zih.jpg "Omega Plugged into Dock")

**Step 3**: Connect the Omega to your computer through USB.

![Omega plugged into USB](//i.imgur.com/0FImt9qh.jpg "Omega plugged into USB")

**Step 4**: Turn on the Omega using the switch.

![Turn on the Omega](//i.imgur.com/gupcwsSh.jpg "Turn on the Omega")

**Step 5**: Wait for the amber LED to stop blinking, indicating that the Omega has booted up.

![Omega is on](//i.imgur.com/FulDB6zh.jpg "Omega is on")

## Setting up using GUI

**Step 1**: Connect to the Omega’s Access Point.

![Connect to AP](//i.imgur.com/TIsvi2Bh.jpg "Connect to AP")

**Step 2**: Browse to `http://omega-ABCD.local` where ABCD are the same characters from the network name above. 

Alternatively, you can also browse to `http://192.168.3.1`. 

You have now arrived at the Setup Wizard. 
Login with the following information: 
```
username: root
password: onioneer 
```
Follow the wizard to complete the setup of the Omega.

![Browse to Setup Wizard](//i.imgur.com/fJsQ77zh.jpg "Browse to Setup Wizard")

Following the wizard will connect your Omega to a wifi network of your choice, and will update to the latest firmware.


## Setting up using Command Line – OSX

**Step 1**: Download and install the [Silicon Labs CP2102 driver for OS X](https://www.silabs.com/Support%20Documents/Software/Mac_OSX_VCP_Driver.zip).

**Step 2**: Run `ls /dev/tty.*` to see if the USB-to-Serial device can be detected. If the driver is successfully installed, you should be able to see a device with a name similar to `/dev/tty.SLAB_USBtoUART`.

![Check if serial device exists](//i.imgur.com/FLn2p35h.jpg "Check if serial device exists")

**Step 3**: Run `screen /dev/tty.SLAB_USBtoUART 115200` to connect to the Omega’s serial terminal using screen.

![Log in through serial terminal](//i.imgur.com/cGANJefh.jpg "Log in through serial terminal")

**Step 4**: Run `wifisetup` in the serial terminal, and follow the prompt to connect the Omega to your Wi-Fi network.

![Run wifisetup](//i.imgur.com/h21sjzRh.jpg "Run wifisetup")

**Step 5**: Run `oupgrade` in the serial terminal, this will update the Omega to the latest firmware.

The firmware update will take a few minutes, the process will be complete when the Omega reboots.
**Warning: Do not disconnect the Omega from wifi or power during this process!**

**Step 6**: Enjoy!



## Setting up using Command Line – Windows

**Step 1**: Download and install the [Silicon Labs CP2102 driver for Windows](https://www.silabs.com/Support%20Documents/Software/CP210x_VCP_Windows.zip).

**Step 2**: Run Computer Management (Start > Run… > Enter “Computer Management” and press `ENTER`), look for Silicon Labs CP210x USB to UART Bridge under Ports (COM & LPT), and take note of the COM number in bracket.

![Computer Management](//i.imgur.com/0fFBiNNh.jpg "Computer Management")

**Step 3**: Download and install Putty from [[http://the.earth.li/~sgtatham/putty/latest/x86/putty-0.65-installer.exe]].

**Step 4**: Open up PuTTY, select Serial for Connection type, enter the COM number noted down in Step 2 as Serial line, and enter `115200` for the speed.

![Configure PuTTY](//i.imgur.com/jnREOQth.jpg "Configure PuTTY")

**Step 5**: Click on the Open button to connect to the Omega via the serial terminal.

![Log in through serial terminal](//i.imgur.com/d6INMZkh.jpg "Log in through serial terminal")

**Step 6**: Run `wifisetup` in the serial terminal, and follow the prompt to connect the Omega to your Wi-Fi network.

![Run wifisetup](//i.imgur.com/u6E5LGSh.jpg "Run wifisetup")

**Step 7**: Run `oupgrade` in the serial terminal, this will update the Omega to the latest firmware.

The firmware update will take a few minutes, the process will be complete when the Omega reboots.
**Warning: Do not disconnect the Omega from wifi or power during this process!**

**Step 8**: Enjoy!



## Setting up using Command Line – Linux

**Step 1**: Check if the serial drivers are already installed.

Some modern Linux Distros already have the required serial drivers installed. Run `modinfo cp210x` on the command line, if it outputs several lines of information, the driver is already installed and you can skip ahead to **Step 4**. 

If the output is something along the lines of 
```
modinfo: ERROR: Module cp210x not found.
```
the driver will need to be installed. Continue to **Step 2**.


**Step 2**: Download and install the Silicon Labs CP2102 driver source files.

For Linux kernel **3.x.x and higher**: [[https://www.silabs.com/Support%20Documents/Software/Linux_3.x.x_VCP_Driver_Source.zip]].
For Linux kernel **2.6.x**: [[https://www.silabs.com/Support%20Documents/Software/Linux_3.x.x_VCP_Driver_Source.zip]].

**Step 3**: Build and install the driver.

*For Ubuntu/Debian*:

Unzip the archive.

`cd` into the unzipped directory.

Compile the driver with `make`.

```bash
$ sudo cp cp210x.ko /lib/modules/<kernel-version>/kernel/drivers/usb/serial/
$ sudo insmod /lib/modules/<kernel-version>/kernel/drivers/usb/serial/usbserial.ko
$ sudo insmod cp210x.ko
$ sudo chmod 666 /dev/ttyUSB0
```

*For RedHat/CentOS*:

```bash
$ sudo yum update kernel* //need to update the kernel first otherwise your header won't match
$ sudo yum install kernel-devel kernel-headers //get the devel and header packages
$ sudo reboot //your build link should be fixed after your system come back
```

Unzip the archive.

`cd` into the unzipped directory.

Compile the driver with `make`.

```bash
$ sudo cp cp210x.ko /lib/modules/<kernel-version>/kernel/drivers/usb/serial
$ sudo insmod /lib/modules/<kernel-version>/kernel/drivers/usb/serial/usbserial.ko
$ sudo insmod cp210x.ko
$ sudo chmod 666 /dev/ttyUSB0
```


**Step 4**: Run `ls /dev/ttyUSB*` to see if the USB-to-Serial device can be detected. If the driver is successfully installed, you should be able to see a device with a name similar to `/dev/ttyUSB0`.

![Check if serial device exists](//i.imgur.com/p1OwSE6h.png "Check if serial device exists")

**Step 5**: Run `screen /dev/tty.SLAB_USBtoUART 115200` to connect to the Omega’s serial terminal using screen.

![Log in through serial terminal](//i.imgur.com/sENEIX8h.png "Log in through serial terminal")

**Step 6**: Run `wifisetup` in the serial terminal, and follow the prompt to connect the Omega to your Wi-Fi network.

![Run wifisetup](//i.imgur.com/qou4iAmh.png "Run wifisetup")

**Step 7**: Run `oupgrade` in the serial terminal, this will update the Omega to the latest firmware.

The firmware update will take a few minutes, the process will be complete when the Omega reboots.
**Warning: Do not disconnect the Omega from wifi or power during this process!**

**Step 8**: Enjoy!
