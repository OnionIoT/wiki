# Generating QR Codes to Display on the OLED Expansions

QR Codes are essentially two dimensional barcodes that can easily be scanned with any camera and a little bit of processing power. The average smartphone will make short work of any QR code it comes across.

![Onion QR Code](http://i.imgur.com/0Ef3def.png)

In this tutorial, we'll go through how you can use Python to encode text into a QR Code and display it on your OLED Expansion.


[[_TOC_]]


[//]: # (Overview)

# Overview 

Tutorial Difficulty:

**Beginner**

Time Required:

**10 minutes**

Required Materials:
* The Omega
* The Expansion Dock
* OLED Expansions

Useful Experience:
* Using the OLED Expansion
* Using Python



[//]: # (What are QR Codes?)

# What are QR Codes?

As mentioned earlier, QR codes (Quick Response Code) are essentially two-dimensional (or matrix) barcodes. They're an evolution of the ordinary barcode in every sense of the word: the data storage capacity is much larger, they can be processed and read by almost any imaging device, and error correction is built-in so even partial codes can be scanned.

The QR code data encoding algorithm defines a number of 'versions' that increase in size and store increasing amounts of data. The size (version) of a generated QR code depends on the amount of data to be encoded. Increasing the error correction capability will decrease the storage capacity due to redundancy pixels being added.

![QR Code Versions](http://i.imgur.com/mPR6X8i.png)

QR Codes can be used in the same way as traditional barcodes, but with the advantages they have over barcodes, there's a variety of clever ways they can be used:
* Holding URLs that will take you to the site when scanned
* Contact information
* Advertising
* Payment systems 
* Authentication and identification

To learn more about QR codes, visit the [Wikipedia article](https://en.wikipedia.org/wiki/QR_code), it's jam-packed with all sorts of information.




[//]: # (The Fun Part)

# The Fun Part

Ok, here we go! First we'll install some required packages to make everything run smoothly, and then we'll grab the code for this tutorial from GitHub.

All of the code can be found on this GitHub Repo: https://github.com/OnionIoT/oledQrCodeGenerator



[//]: # (The Fun Part: Required Packages)

## Installing Required Packages

We will need to have support for git, Python, and the [Onion OLED Expansion Python Module](https://wiki.onion.io/Documentation/Libraries/OLED-Expansion-Library):
```
opkg update
opkg install git git-http python-light python-codecs pyOledExp
```


[//]: # (The Fun Part: Repo Code)

## Downloading the Code

Now we need to download the Python code that actually does all the work:
```
cd /root
git clone https://github.com/OnionIoT/oledQrCodeGenerator.git
```


[//]: # (The Fun Part: Running the Code)

## Running the Code

Finally, we get to make some QR codes! 
Navigate into the repo directory:
```
cd oledQrCodeGenerator
```

And run the program, the argument to the script is the text that will be encoded in the QR code pattern:
```
root@Omega-18C2:~/oledQrCodeGenerator# python main.py 'Wow, my first QR Code'
> Encoding 21 characters
> Generated QR Code: 31x31 pixels
> Doubled QR Code size: 62x62
> Initializing display
> Setting display mode to inverted
> Writing buffer data to display
```

This will encode the data and display the resulting QR code on the OLED Expansion:

![QR Code on OLED Exp](http://i.imgur.com/yhLiYEN.jpg)



[//]: # (The Fun Part: Running the Code: Program Details)

### Program Details

Behind the scenes, the Python code does the following:
* Encodes the input text into a matrix representing the QR Code
  * The size of the QR code is based on the amount of input text
* Converts the QR code matrix into data that can be displayed on the OLED
* Displays the resulting image on the OLED display
  * Performs display initialization
  * Inverts the display colours
  * Displays the generated image file

An additional feature was added for easier scanning: if the QR code is small (less than half the height of the OLED display), the image will be doubled in size so that each QR code pixel shows up as four pixels on the OLED display.

The default generated QR code will be a Version 3 code with the Low error correction setting and a one-pixel border, creating a code that is 31x31 pixels. If the amount of text to be encoded cannot fit in a Version 3 code, the program will select the next version that will fit the amount of data to be encoded.



[//]: # (The Fun Part: Using the code as a Python Module)

## Using the code as a Python Module

The `oledQrCodeGenerator` code can also be imported as a module into your own Python projects! 

The `dispQrCode()` function will perform the same actions described above.


**Example Code**

A small example showing how to use this module:
``` python
import oledQrCodeGenerator

print 'Now using the oledQrCodeGenerator'
oledQrCodeGenerator.dispQrCode('Hello!')

print 'All done!'
```

Note that your Python script will have to be in the same directory as `oledQrCodeGenerator` in order for it to work properly. 



[//]: # (Reading QR Codes)

# Reading QR Codes

It's no fun to just display QR codes and not be able to read them, right?

Don't worry, your smartphone is perfectly capable of reading the code from the OLED:
* On Android, we've used the [QR Code Reader](https://play.google.com/store/apps/details?id=tw.mobileapp.qrcode.banner) and [QR Barcode Scanner](https://play.google.com/store/apps/details?id=appinventor.ai_progetto2003.SCAN&hl=en) apps successfully
* On iOS, we've had success with the [QR Reader App](https://itunes.apple.com/us/app/qr-code-reader-and-scanner/id388175979?mt=8)

For QR codes that encode a lot of text, your phone might take a little while longer to scan the code. Trial and error works best in this scenario: try moving your phone to different distances and angles from the OLED.

Happy hacking!



[//]: # (Acknowledgements)

# Acknowledgements

The code in the `qrcode` directory is a stripped-down version of lincolnloop's `python-qrcode` repo:
https://github.com/lincolnloop/python-qrcode

The rest of the code is home grown by the Onion team :)

