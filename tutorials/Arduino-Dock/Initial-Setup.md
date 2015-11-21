# Arduino Dock Initial Setup
Before you can get started with the Arduino Dock, there are a few thigns that need to be done on your computer and then on the Arduino Dock.

These steps will only need to be carried out once, and then you'll be set to go.

## Computer setup
These steps ensure the Omega and Arduino Dock will be detected wirelessly and correctly work with your computer.

### Arduino IDE 

### Installation
Install the latest Arduino IDE from the good folks over at [Arduino](https://www.arduino.cc/en/Main/Software). We did all of our testing using Version 1.6.6.

### Modification of boards.txt
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

### Installation of Onion Arduino Library
The Onion Arduino Library provides the required setup and functions required for the Arduino Dock to operate correctly.

#### Installing the library
* We're working on a way of stream-lining this process, stay tuned!*
Follow these steps to install the library:
* Clone the [Onion Arduino Library repo](https://github.com/OnionIoT/Onion-Arduino-Library) to your computer
* Open the Arduino IDE
  * Open the Sketch Menu, and then Include Library -> Add .ZIP Library
  ![Add library](https://i.imgur.com/na7wNcY.png)
  * Navigate to where the Onion Arduino Library repo has been cloned
  * Select Onion-Arduino-Library.zip
* The library is now installed, it can be included in your sketches by opening the Sketch menu -> Include Library -> Onion



## Arduino Dock Setup
The ATmega chip on the Arduino Dock needs to be burned with a new bootloader to work with the Omega properly. This only has to be done once and your Arduino Dock will be good to go. The new bootloader allows the ATmega to be flashed via the I2C connection with the Omega. There are a few methods to flash the bootloader, they are outlined below.


### Using the Omega + Expansion Dock as a Programmer
The Omega can actually be used to program the bootloader

#### Required Components
* An Omega
* An Expansion Dock
* The target Arduino dock
* Six jumper wires (Male-to-male)
  * can just be six pieces of wire with the insulation stripped off at the end

#### Wiring
Plug the Omega into the Expansion Dock and provide power via Micro USB. 

The Expansion Dock ports need to be connected to specific Arduino Dock ports for the programming to work.
Follow the table below to wire the Omega to the Arduino Dock, ensure that you connect the 5V and GND pins last just as a precaution. **Note that it's very important that the Omega and the Arduino Dock share a common ground!**

| Function  | Expansion Dock GPIO | Arduino Dock Pin |
|-----------|---------------------|------------------|
| SPI MISO  | 1                   | 12               |
| SPI SCK   | 6                   | 13               |
| SPI RESET | 7                   | Reset            |
| SPI MOSI  | 19                  | 11               |
| VCC       | 5V                  | 5V               |
| GND       | GND                 | GND              |

![Programmer Connections](http://i.imgur.com/cWZn7YI.jpg)
![Programmer Connections with Power](http://i.imgur.com/qOQCPV2.jpg)


#### Programming
*Ensure you are on Omega firmware b220 or later.*

Connect to the Omega's terminal and run the following command:
```
sh /usr/bin/arduino-dock flash bootloader
```
Unless there were errors in the programming sequence, the Arduino Dock should now have the Bootloader installed and the amber LED next to the DC connector should be flickering.

#### Summary
Once this is complete, the extra wires and the Expansion Dock are no longer required! 
Check out the [[guide on using the Arduino Dock|Using the Arduino Dock]] for what to do next.


### Using an Arduino as a Programmer
Stil working on this one, stay tuned!