# Omega Hardware 

The Onion Omega is a hardware development platform with built-in WiFi and a full Linux Operating System.

![Omega in Hand](http://i.imgur.com/PX5vCMz.jpg)

The Omega has 18 digital GPIOs, support for I2C, SPI, and USB2.0, among other things.

[[_TOC_]]

[//]: # (Tech Specs)

# Technical Specifications


[//]: # (Tech Specs: SoC)

## The Omega's Brain

The Omega's brain and central nervous system are provided by the Qualcomm Atheros AR9331 SoC.

With the SoC and on-board components, the Omega has the following:
* 400 MHz MIPS 24Kc Big-Endian Processor 
* 64MB of DDR2 RAM running at 400 MHz
* 16MB of on-board flash storage
* Support for USB 2.0
* Support for Ethernet at 100 Mbps
* 802.11b/g/ WiFi at 150 Mbps

[//]: # (Tech Specs: Connectivity)

## Connectivity

The Omega also has the following:
* 18 digital GPIOs
* A single Serial UART
* Support for SPI
* Support for I2S


[//]: # (Tech Specs: Power)

## Power 

The Docks receive 5V through the Micro-USB connector. The Docks also have regulators to provide the Omega with a constant 3.3V.

Through observation, we've found the following current draw on the Omega:

| Dock           | Idle Current Draw | Load Current Draw | Peak Current Draw |
|----------------|-------------------|-------------------|-------------------|
| Expansion Dock | 120 mA            | 150 mA            | 180 mA            |
| Mini Dock      | 170 mA            | 200 mA            | 220 mA            |
| Arduino Dock   | 110 mA            | 140 mA            | 180 mA            |

*Note that measurements above meant as guidelines, they are observational in nature and have not been rigiously confirmed.*


[//]: # (Tech Specs: Datasheet)

## SoC Datasheet

The [AR9331 Datasheet](ar9331_datasheet.pdf) has all of the technical specifications your heart might desire.



[//]: # (Hardware)

# The Hardware

The Omega Docks are powered through the **Micro-USB** connector. Use a micro-USB cable (same cable used to charge Andriod phones) to connect your Omega to a computer or a wall charger.


[//]: # (Hardware: Reset Button)

## Reset Button

A Reset Button can be found on the Expansion Dock and Mini Dock:

![Mini Dock + Exp Dock Reset Button Location](http://i.imgur.com/5ZN4y2d.png)

For reference, the Reset Button is connected to Omega's GPIO11.


### Reboot

Momentarily pressing the reset button and letting go will initiate a reboot of the Omega OS.

### Factory Restore

Pressing and holding the reset button for 10 seconds and releasing will trigger a factory restore.

**Warning:** This will reset your Omega to the default filesystem of the last firmware update, this **will delete ALL of your data!**


[//]: # (Hardware: Omega LED)

## Omega LED

The Omega has an on-board LED that indicates it's status. By default, the LED will flash while the Omega is booting, and will turn solid once the boot sequnce is complete. It will remain solid as long as the Omega is powered on.

![Omega LED](http://i.imgur.com/FulDB6z.jpg)


### Controlling the LED

The LED is connected to Omega's GPIO27, however, fine-grain control of the LED is available through the Linux sysfs interface: `/sys/class/leds/onion\:amber\:system/`


**Check Available Modes**

To read the available modes, run the following in the command line:
```
root@Omega-0100:~#  cat /sys/class/leds/onion\:amber\:system/trigger
none timer [default-on] netdev transient gpio heartbeat morse oneshot usbdev
```

The currently selected mode will be surrounded with square brackets.

**Changing the Mode**

To change the mode, run the following command:
```
echo MODE > /sys/class/leds/onion\:amber\:system/trigger
```

Where `MODE` is one of the options seen above.



[//]: # (Hardware: GPIOs)

## GPIOs

The Omega has 18 digital GPIOs, running at 3.3V.

The pinout is as follows:

![Omega Pinout](http://i.imgur.com/Oug8I8H.png)

Two pins are designated for hardware I2C and one for software reset of the Omega.


### Expansion Dock Pinout

The Expansion Dock has a 30 pin header that breaks out a variety of the Omega's pins:

![Expansion Dock Pinout](http://i.imgur.com/3QPA9Ik.jpg)


### Electrical Characteristics

The best electrical characteristics we could find and test:

| Characteristic          | Min  | Max        |
|-------------------------|------|------------|
| Output High Voltage (V) | 2.44 | ---        |
| Output Low Voltage (V)  | ---  | 0.1        |
| Input High Voltage (V)  | 0.7  | 5 (tested) |
| Input Low Voltage (V)   | 0.3  | ---        |

Maximum drive: 24 mA

Pullup/Pulldown resistance: 200 kΩ



[//]: # (Hardware: I2C)

## I2C

The Omega supports hardware I2C using two GPIOs:

| I2C         | GPIO |
|-------------|------|
| SCL - Clock | 20   |
| SDA - Data  | 21   |

The I2C interface can be accessed in the Linux filesystem through `/dev/i2c-0`


[//]: # (Hardware: USB)

## USB

The Omega, like all computers, supports USB devices.

The Expansion Dock, Mini Dock, and Arduino Dock all have USB Host connectors. A large variety of devices are supported out of the box, including webcams, keyboards, flash drives, hard drives, etc.


[//]: # (Hardware: UART)

## UART

The Omega has a two wire Serial UART bus, with a TX and RX pin. 

On the Expansion Dock and Mini Dock, there is a USB-to-Serial chip that allows the Omega's serial terminal to be accessed on your computer through a USB cable.

On the Arduino Dock, the Serial port is connected to the on-board ATmega microcontroller. Thereby the serial terminal is not available when using the Arduino Dock.

The Linux interface is `/dev/ttyATH0` and the default baudrate is 115200.



