# Using the Arduino Dock

[[_TOC_]]

The Onion Arduino Dock is meant to be an interface between the Omega and the popular ATmega microcontroller, allowing you to bring the flexibility of the Omega into the world of microcontrollers.

![Onion Arduino Dock](http://i.imgur.com/pqCNKt7.jpg)

## The Hardware
It is essentially our (supercharged) version of an Arduino Uno R3 board, since they share the same microcontroller, the ATmel ATmega328P microcontroller (MCU), and have identical pin layouts. This allows users to use any Arduino shields in conjunction with the Arduino Dock and the Omega.

The Dock also includes an In-Circuit Serial Programming (ICSP) header to break out the SPI pins which can be used to program the IC. Additionally, there is a USB-host port that is connected to the Omega.

## Initial Setup
Before you can get started with the Arduino Dock, there are a few things that need to be done on your computer and then on the Arduino Dock.

### Computer setup
These steps ensure the Omega and Arduino Dock will be detected wirelessly and correctly work with your computer.

#### Arduino IDE 

#### Installation
Install the latest Arduino IDE from the good folks over at [Arduino](https://www.arduino.cc/en/Main/Software). We did all of our testing using Version 1.6.6.

#### Modification of boards.txt
The regular Arduino Uno has no way of communicating with your computer via WiFi, but the Omega provides the Arduino Dock with a means of wireless communication, so we'll have to let the Arduino software know! 
This requires a one line addition to the `boards.txt` file in your Arduino IDE installation.

Open the boards.txt file in any text editor. The following table outlines where the `boards.txt` file can be found:
| Operating System | Location of `boards.txt`                                                  |
|------------------|---------------------------------------------------------------------------|
| OS X             | `/Applications/Arduino.app/Contents/Java/hardware/arduino/avr/boards.txt` |
| Windows          | `C:\Program Files (x86)\Arduino\hardware\arduino\avr\boards.txt`          |

Once it's open, add the following line:
```
uno.upload.via_ssh=true
```
Doesn't matter where you add it, you can nestle it with the other `uno.` settings if you like.

#### Installation of Onion Arduino Library
The Onion Arduino Library provides the required setup and functions required for the Arduino Dock to operate correctly.

##### Installing the library
* We're working on a way of stream-lining this process, stay tuned!*
Follow these steps to install the library:
* Clone the [Onion Arduino Library repo](https://github.com/OnionIoT/Onion-Arduino-Library) to your computer
* Open the Arduino IDE
  * Open the Sketch Menu, and then Include Library -> Add .ZIP Library
  ![Add library](https://i.imgur.com/na7wNcY.png)
  * Navigate to where the Onion Arduino Library repo has been cloned
  * Select Onion-Arduino-Library.zip
* The library is now installed, it can be included in your sketches by opening the Sketch menu -> Include Library -> Onion



### Arduino Dock Setup
The ATmega chip on the Arduino Dock needs to be burned with a new bootloader to work with the Omega properly. This only has to be done once and your Arduino Dock will be good to go. There are a few methods to flash the bootloader, they are outlined below.

#### Using the Omega as a Programmer
The Omega can actually be used to program the bootloader

##### Required Components
* An Omega
* An Expansion Dock
* The target Arduino dock
* Six jumper wires (Male-to-male)
  * can just be six pieces of wire with the insulation stripped off at the end

##### Wiring
Plug the Omega into the Expansion Dock and provide power via Micro USB. 

The Expansion Dock ports need to be connected to specific Arduino Dock ports for the programming to work.
Follow the table below to wire the Omega to the Arduino Dock, ensure that you connect the 5V and GND pins last just as a precaution.
| Function  | Expansion Dock GPIO | Arduino Dock Pin |
|-          |-                    |-                 |
| SPI MISO  | 1                   | 12               |
| SPI SCK   | 6                   | 13               |
| SPI RESET | 7                   | Reset            |
| SPI MOSI  | 19                  | 11               |
| VCC       | 5V                  | 5V               |
| GND       | GND                 | GND              |

![Programmer Connections](http://i.imgur.com/cWZn7YI.jpg)
![Programmer Connections with Power](http://i.imgur.com/qOQCPV2.jpg)


##### Programming
*Ensure you are on Omega firmware b220 or later.*

Connect to the Omega's terminal and run the following command:
```
sh /usr/bin/arduino-dock flash bootloader
```
Unless there were errors in the programming sequence, the Arduino Dock should now have the Bootloader installed and the amber LED next to the DC connector should be flickering.

#### Using an Arduino as a Programmer
Stil working on this one, stay tuned!


## Using the Arduino Dock
![Omega + Arduino Dock](http://i.imgur.com/B47pqlW.jpg)

### Flashing Sketches
Now we get to the fun part, flashing sketches to the ATmega chip!

Ensure that all sketches include the Onion Arduino Library and that an Onion object is instantiated in the `setup()` function. If this is not done, the MCU_RESET button on the Omega will have to be pressed before uploading the Sketch.

#### Using the Arduino IDE
Thanks to the setup you did on your computer and the Arduino Dock, you can actually use the Arduino IDE on your computer to flash Sketches to the Arduino Dock, so long as your computer and the Omega on your Arduino Dock are on the same wifi network.

In the Arduino Tools menu, select "Arduino/Guinuino Uno" for the Board, and your Omega-ABCD hostname as the Port:
![Arduino IDE Tools->Port menu](http://i.imgur.com/1xAEBmT.png)

If your Omega does not show up in the Port menu as a network port, restart the Arduino and wait for 30 seconds:

When your sketch is ready, hit the Upload button. Once the sketch is compiled, it will require your Omega password to upload:
![Arduino IDE Uploading Sketch](http://i.imgur.com/UDXIDVL.png)

The IDE actually creates an SSH connection with the Omega to transfer the compiled hex file, and the Omega with then flash the ATmega chip via I2C!
(If your previous sketch did not include the Onion Arduino Library you will have to press the MCU_RESET button on the Arduino Dock right after entering your password in order to flash successfully)

Once the upload completes, the info screen will show something along the lines of:
![Arduino IDE Upload Done](http://i.imgur.com/oPOB4Vl.png)

The ATmega chip is now running your sketch, enjoy!

#### Using the Omega
The Omega can actually flash the ATmega chip as well. This is handy if the Arduino IDE cannot detect your Omega as a Network Port due to any connection/setup issues.

First, enable verbose output during compilation in the Arduino IDE Preferences:
![Arduino IDE Preferences](http://i.imgur.com/A6uXT6Y.png)

Hit the verify button to compile the sketch, once it's complete you will have to scroll to the right to find the path to the compiled hex file:
![Arduino IDE Compiled Hex file](http://i.imgur.com/QEiDwu8.png)

Copy this path and then transfer the file to your Omega.

I personally like to use rsync to transfer files from my computer to the Omega, the rsync command on my computer to transfer to the Omega is as follows:
```
rsync -va <path to compiled sketch> root@omega-abcd.local:/root/<hex name>
```
As an example:
```
rsync -va /var/folders/55/7v7wrbcs57jflv83zgz8v67m0000gn/T/builddf059f74b6cb24ac573e131020699c2d.tmp/blink2.ino.hex root@omega-1302.local:/root/blink2.hex
```
Once the hex file is on your Omega, you can flash it to the ATmega chip from the Omega's terminal:
```
sh /usr/bin/arduino-dock flash <hex file>
```
For example:
```
# sh /usr/bin/arduino-dock flash /root/blink2.hex 
> Flashing application '/root/blink2.hex' ...
device         : /dev/i2c-0       (address: 0x29)
version        : TWIBOOTm328pv2.1??x (sig: 0x1e 0x95 0x0f => AVR Mega 32p)
flash size     : 0x7800 / 30720   (0x80 bytes/page)
eeprom size    : 0x0400 /  1024
writing flash  : [**************************************************#?] (3210)
verifying flash: [**************************************************#?] (3210)
> Done
```
The sketch has been flashing and is running on the Arduino Dock, enjoy!

(If your previous sketch did not include the Onion Arduino Library you will have to press the MCU_RESET button on the Arduino Dock just before pressing Enter for the arduino-dock command)

### The Onion Arduino Library
The library provides some essential functionality for the Arduino Dock

#### Basic Setup in Sketch
To add the Onion library to your Sketch in the IDE, open the Sketch menu, navigate to Include Library -> Onion
![Add the Onion Library](https://i.imgur.com/MjYaLTO.png)
Once the library has been added, create a global instance of the Onion object. 
Add the following line on the global scope:
```
Onion* onionSetup;
```

To initialize and activate the library functions, add the following line to the `setup()` function in the sketch:
```
  onionSetup = new Onion;
```

Check out our blink2 example to see this in action. The example can be accessed in the Arduino IDE through File -> Examples -> Onion -> blink2

#### Library Functionality
For now the library setups up the ATmega to join the I2C bus. It then listens on the bus for a command from the Omega to go into Reset so a new Sketch can be flashed.

As described above, if the Onion library is not used in your sketch, the MCU_RESET button on the Arduino Dock will have to be manually pressed right before a new sketch is uploaded.

We're working on more functionality for this library, namely using the Serial Port as a bridge between the Omega and ATmega chip! Stay tuned!

#### Using I2C in a Sketch
Right now the Onion Library joins the I2C bus with an address of 0x08 and listens for a write of 0xde 0xad (get it? 'dead' ha ha ha) when resetting. If you need a Sketch to use I2C, make sure to build this functionality into your I2C handling to comply with the Arduino Dock flashing procedure. 

