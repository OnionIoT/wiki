# Controlling Neopixels

The Omega + Arduino Dock can be used to control Neopixels, with the Omega sending the commands and colour codes, and the Arduino Dock acting as a communication channel.

![Omega+Arduino Dock w/ Neopixel Array](http://i.imgur.com/l5oVLdi.jpg)

[[_TOC_]]


[//]: # (Arduino Dock Setup)

## Arduino Dock Setup

[Download and install](Initial-Setup#computer-setup_installation-of-onion-arduino-library) the latest Onion Arduino Library, and flash a sketch to the Arduino Dock that has the Onion Library [enabled and setup](Using-the-Arduino-Dock#the-onion-arduino-library_basic-setup-in-sketch).

The Arduino Dock is now ready to accept commands from the Omega that will control Neopixels. The Library code handles all of the Neopixel commands, **no additional changes to the Arduino Sketch are required to control the Neopixels**.



[//]: # (Methods to Control the Neopixels)

## Methods to Control the Neopixels

Any connected Neopixels can be controlled with the following methods:
* Command line tool on the Omega
* C++ Library
* C Library
* Python Module

The latter three options allow you to write your own programs to interact with Neopixels.


[//]: # (Methods to Control the Neopixels: Programming Flow)

### Programming Flow

All of methods listed above follow the same programming flow:
* Initialize the Neopixels; set the Arduino Dock pin to be used as data output, set the number of Neopixels
* Set the colour of one or several pixels
* Display the queued up colour changes



[//]: # (Omega Command Line Tool)

## Omega Command Line Tool

The `neopixel-tool` utility provides a method to control Neopixels from the command line.


[//]: # (Omega Command Line Tool: Installation)

### Installation

To install the `neopixel-tool` utility:
```
opkg update
opkg install neopixel-tool
```


[//]: # (Omega Command Line Tool: Usage)

### Usage

The following describes how to use the command line tool.


[//]: # (Omega Command Line Tool Usage: Init)

#### Initialization

The Arduino Dock must be told which Port to use to communicate with the Neopixels, and how many Neopixels are to be programmed:
```
neopixel-tool -i -p <pin> -l <number of neopixels>
```

**Arguments**

The **pin** argument indicates which Arduino Dock pin is connected to the Neopixel Data In. If no pin  is specified, the tool will assume Pin 6 is being used to control the Neopixels.

The **number of neopixels** argument indicates how many neopixels there are in the conencted Neopixel strip, array, or matrix. If the number is not specified, the tool will assume there are 32 individual pixels.


**Examples**

Initialize Arduino Dock to use pin 5 for a strip with 64 pixels
```
neopixel-tool -i -p 5 -l 64
```


[//]: # (Omega Command Line Tool Usage: Setting a Single Pixel)

#### Set the Colour of a Single Pixel

Single pixels can be programmed with a specific hex colour code:
```
neopixel-tool set <pixel> <colour code>
```

Note that the above command will queue up the colour change, to immediately show the colour change:
```
neopixel-tool -s set <pixel> <colour code>
```

**Arguments**

The pixel specifies which Neopixel in your strip or array to program, with the first pixel being 0.

The colour code is a 24-bit hex colour, with 8-bits representing the intensity of each colour component. Experiment with a [colour wheel](http://www.w3schools.com/tags/ref_colorpicker.asp) to find your favourite colours.


**Examples**

Set the first pixel in the strip to be red:
```
neopixel-tool set 0 0xff0000
```

Set the fifth pixel in the strip to be blue:
```
neopixel-tool set 4 0x0000ff
```

Set the second pixel in the strip to be green, and to show this change immediately:
```
neopixel-tool -s set 1 0x00ff00
```



[//]: # (Omega Command Line Tool Usage: Setting Many Pixels at Once)

#### Set the Colour of Many Pixels

Multiple pixels can be programmed at once by writing a large buffer of colours:
```
neopixel-tool buffer <pixel colour string>
```


Note that the above command will queue up the colour change, to immediately show the colour change:
```
neopixel-tool -s buffer <pixel colour string>
```

**Arguments**

The pixel colour string specifies the colours for each pixels, starting with pixel 0. Imagine it like stringing together a number of commands that set single pixels: 24-bits for each pixel.


**Examples**

Set the first three pixels to red, green, and then blue:
```
neopixel-tool buffer ff000000ff000000ff
```

Set the first five pixels to: yellow, pink, light blue, purple, and lime
```
neopixel-tool buffer ffff00ff66ff3399ffcc00ccbfff80
```



[//]: # (Omega Command Line Tool Usage: Setting the Brightness)

#### Set the Maximum Pixel Brightness

Neopixels can be programmed with to maximum brightness:
```
neopixel-tool -b <brightness>
```


Note that the above command will queue up the brightness change, to immediately show the colour change:
```
neopixel-tool -sb <brightness>
```

**Arguments**

The **brightness** argument specifies the maximum colour intensity for all colour components. The range is **0 to 255**


**Examples**

Set the maximum brightness:
```
neopixel-tool -b 255
```

Set to half brightness:
```
neopixel-tool -b 127
```

Set to quarter brightness, taking effect right away:
```
neopixel-tool -sb 63
```



[//]: # (Omega Command Line Tool Usage: Show all queued pixel changes)

#### Show Queued Colour Changes

Any commands to set the colours of pixels will not be immediately displayed on the Neopixels (unless they are run with the -s option).

To show all queued colour changes:
```
neopixel-tool -s
```


[//]: # (Using the Libraries)

## Using the Libraries

The C and C++ Libraries and Python module can be used in your own programs to control Neopixels.
For more information, see [this guide](../Libraries/Arduino-Dock-Neopixel-Library).



