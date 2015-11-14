# Get Started with the Onion Omega

[[_TOC_]]

## Preparing the Hardware

**Step 1**: Unpack the Omega and Dock from the boxes.

![Omega + Dock](https://i.imgur.com/tKs4wRWh.jpg "Omega + Dock")

**Step 2**: Connect the Omega to the Dock.

![Omega plugged into Dock](https://i.imgur.com/rek12Zih.jpg "Omega Plugged into Dock")

**Step 3**: Connect the Omega to your computer through USB.

![Omega plugged into USB](https://i.imgur.com/0FImt9qh.jpg "Omega plugged into USB")

**Step 4**: Turn on the Omega using the switch.

![Turn on the Omega](https://i.imgur.com/gupcwsSh.jpg "Turn on the Omega")

**Step 5**: Wait for the amber LED to stop blinking, indicating that the Omega has booted up.

![Omega is on](https://i.imgur.com/FulDB6zh.jpg "Omega is on")

## Setting up using GUI

**Step 1**: Connect to the Omega’s Access Point.

![Connect to AP](https://i.imgur.com/TIsvi2Bh.jpg "Connect to AP")

**Step 2**: Browse to ```http://omega-ABCD.local``` where ABCD are the same characters from the network name above. Alternatively, you can also browse to ```http://192.168.3.1```. You have now arrived at the Setup Wizard. Login with the following information: username: root password: onioneer Follow the wizard to complete the setup of the Omega.

![Browse to Setup Wizard](https://i.imgur.com/fJsQ77zh.jpg "Browse to Setup Wizard")

## Setting up using Command Line – OSX

**Step 1**: Download and install the Silicon Labs CP2102 driver from [[https://www.silabs.com/Support%20Documents/Software/Mac_OSX_VCP_Driver.zip]].

**Step 2**: Run ```ls /dev/tty.*``` to see if the USB-to-Serial device can be detected. If the driver is successfully installed, you should be able to see a device with a name similar to ```/dev/tty.SLAB_USBtoUART```.

![Check if serial device exists](https://i.imgur.com/FLn2p35h.jpg "Check if serial device exists")

**Step 3**: Run ```screen /dev/tty.SLAB_USBtoUART 115200``` to connect to the Omega’s serial terminal using screen.

![Log in through serial terminal](https://i.imgur.com/cGANJefh.jpg "Log in through serial terminal")

**Step 4**: Run ```wifisetup``` in the serial terminal, and follow the prompt to connect the Omega to your Wi-Fi network.

![Run wifisetup](https://i.imgur.com/h21sjzRh.jpg "Run wifisetup")

## Setting up using Command Line – Windows

**Step 1**: Download and install the Silicon Labs CP2102 driver from [[https://www.silabs.com/Support%20Documents/Software/CP210x_VCP_Windows.zip]].

**Step 2**: Run Computer Management (Start > Run… > Enter “Computer Management” and press ```ENTER```), look for Silicon Labs CP210x USB to UART Bridge under Ports (COM & LPT), and take note of the COM number in bracket.

![Computer Management](https://i.imgur.com/0fFBiNNh.jpg "Computer Management")

**Step 3**: Download and install Putty from [[http://the.earth.li/~sgtatham/putty/latest/x86/putty-0.65-installer.exe]].

**Step 4**: Open up PuTTY, select Serial for Connection type, enter the COM number noted down in Step 2 as Serial line, and enter ```115200``` for the speed.

![Configure PuTTY](https://i.imgur.com/jnREOQth.jpg "Configure PuTTY")

**Step 5**: Click on the Open button to connect to the Omega via the serial terminal.

![Log in through serial terminal](https://i.imgur.com/d6INMZkl.jpg "Log in through serial terminal")

**Step 6**: Run ```wifisetup``` in the serial terminal, and follow the prompt to connect the Omega to your Wi-Fi network.

![Run wifisetup](https://i.imgur.com/u6E5LGSh.jpg "Run wifisetup")

## Setting up using Command Line – Linux

**Step 1**: Download and install the Silicon Labs CP2102 driver source files.

For Linux kernel 3.x.x and higher: [[https://www.silabs.com/Support%20Documents/Software/Linux_3.x.x_VCP_Driver_Source.zip]].
For Linux kernel 2.6.x: [[https://www.silabs.com/Support%20Documents/Software/Linux_3.x.x_VCP_Driver_Source.zip]].

**Step 2**: Build and install the driver.

*For Ubuntu/Debian*:

Unzip the archive.

```cd``` into the unzipped directory.

Compile the driver with ```make```:

```
cp cp210x.ko to /lib/modules/<kernel-version>/kernel/drivers/usb/serial/<kernel-version>
insmod /lib/modules/<kernel-version>/kernel/drivers/usb/serial/usbserial.ko
insmod cp210x.ko
sudo chmod 666 /dev/ttyUSB0
```

*For RedHat/CentOS*:

```
yum update kernel* //need to update the kernel first otherwise your header won't match
yum install kernel-devel kernel-headers //get the devel and header packages
reboot //your build link should be fixed after your system come back
```

Unzip the archive.

```cd``` into the unzipped directory.

Compile the driver with ```make```:

```
cp cp210x.ko to /lib/modules/<kernel-version>/kernel/drivers/usb/serial
insmod /lib/modules/<kernel-version>/kernel/drivers/usb/serial/usbserial.ko
insmod cp210x.ko
sudo chmod 666 /dev/ttyUSB0
```

**Step 3**: Run ```ls /dev/ttyUSB*``` to see if the USB-to-Serial device can be detected. If the driver is successfully installed, you should be able to see a device with a name similar to ```/dev/ttyUSB0```.

![Check if serial device exists](https://i.imgur.com/p1OwSE6h.png "Check if serial device exists")

**Step 4**: Run ```screen /dev/tty.SLAB_USBtoUART 115200``` to connect to the Omega’s serial terminal using screen.

![Log in through serial terminal](https://i.imgur.com/sENEIX8h.png "Log in through serial terminal")

**Step 5**: Run ```wifisetup``` in the serial terminal, and follow the prompt to connect the Omega to your Wi-Fi network.

![Run wifisetup](https://i.imgur.com/qou4iAmh.png "Run wifisetup")
