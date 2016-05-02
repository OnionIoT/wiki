# Omega Firmware Changelog
Log of all changes made to the Onion Omega OpenWRT Firmware

[[_TOC_]]

# Version Syntax

The Omega Firmware will be in the following format:
```
A.B.C bXYZ
```

The individual parts correspond to the following:
* A is the ULTRA version
* B is the Major version
* C is the Minor version
* XYZ is the build number

For example:
```
0.0.6 b264
```

The build number will continuously increment over the course of development.

# Versions
Definining the differences in each version change

## 0.1.1
**Brin M1 Release.**

Firmware updates for stable Onion Cloud support!

* Stable device client 
* Support for GPS Expansion
* Onion sh library fix: no longer creating blank log files

## 0.1.0
**Brin A2 Release.**

Firmware includes support for the Onion Cloud!

Console Updates:
* Settings App now includes Cloud Settings Tab, used to register the device with the Cloud, and then to view the cloud registration info
* OLED App can now save converted image files to the Omega

## 0.0.8
**Ando A1 Release.**

Added the following features to the firmware:
* Bluetooth audio support using Onion customized packages
* Network Manager that will automatically try to connect to recognized networks
* Fix for Node.JS and NPM where HTML requests were erroneously resolving to IPv6

Console Updates:
* Added Programming Calculator app
* Added Webcam App
* Overhauled WiFi settings apps 

## 0.0.7
**Modern Node Release.**

Added kernel changes to support NodeJs v4.3.1

Updates to Onion Drivers


## 0.0.6
**Ando J1 Console Release.**

Added Resistor Calculator app.

Implemented universal notification system.

Added error notification to I2C app to indicate Expansion cannot be found.

Settings app update: added loading icon to wifi setup (sta and ap), added download progress bar to firmware upgrade, added Omega LED blinking before Factory Reset

## 0.0.5
**Ando N2 Console Release.**

Added apps for the PWM, Relay, and OLED Expansions, expanded the GPIO Control App.

Added taskbar for apps.

## 0.0.4
Early November release of the Console. This series of releases is code-named Ando.

Contains bug fixes, extends functionality of existing apps, added GPIO control app.

## 0.0.3
First release of the Console

## 0.0.2
Post production firmware

## 0.0.1
Initial firmware sent to be flashed at the factory


# Build Notes
Defining the changes in each build. *Note that if a number is missing, that build failed the deployment process.*

## b308
*April 29, 2016*

**Firmware 0.1.1 Release**

Console Updates
* Now at Brin M1 codename
* Settings App will no longer be cached

## b307
*April 28, 2016*

* device-client version upped to 0.5
  * Push out updates that fix connection hang in the event of a network outage

## b306
*April 20, 2016*

* updates to the sh library to fix bug where blank tmp files kept getting created
* updates to fast-gpio to fix blank tmp files bug
* added ubus service to launch processes in the background

## b304
*April 15, 2016*

* device-client version upped to 0.4
  * Push out the updates that fix the curl and threading memory leaks
* Updates to firmware database access scripts to decrease verbosity

## b303
*April 13, 2016*

Build Server related:

Updated firmware database access scripts to match new firmware database web API

## b302
*April 11, 2016*

Added `ogps` to the onion packages repo. 

## b301
*April 10, 2016**

Added script to use with buildroot automated compilation: Checks compile exit codes and retries up to 2 additional times, returns last compile exit code

## b300
*April 8, 2016*

**Version 0.1.0 Firmware**

* Added support for the Onion Cloud
* Updated to latest version of the Console
  * Settings App: Added Cloud Settings tab
  * OLED App can now save converted images to the Omega

## b299
*April 4, 2016*

* New wdb40 version: fixed issue with connecting to SSIDs with spaces

## b297
*April 4, 2016*

* Added `tmux` back into the firmware
* New device-client version for repo: fixed device ID and secret location

## b296
*April 2, 2016*

**Version 0.0.8 Firmware**

Updated to latest version of Console and WDB40

Console:
* Added Calculator and Webcam Apps
* Overhauled the WiFi Settings Apps

WDB40:
* Updated usage instructions

## b295
*April 2, 2016*

* Removed deprecated ubus-intf package (functionality replaced by onion-ubus gpio method, saving space)
* Replaced wifisetup and wifisaint packages with wdb40 network manager
* Updated onion-ubus with new wdb40 install paths

## b294
*April 2, 2016*

* Added Onion customized bluez and pulseaudio packages to Onion Repo
* Modified deployment script to correctly upload images and packages

## b293
*Mar 31, 2016*

Added kmod-input-uinput package into the build.

## b290, b289
*Mar 30, 2016*

* Kernel config update now done by appending to config file from script (instead of overwriting), increased build stability
* Added uClibc toolchain patch that makes web requests default to IPv4 (Required for NodeJS to properly get HTML)
* Updated onion-ubus: modified and fixed gpio method

## b288
*Mar 19, 2016*

Added Onion GPIO Python module package to Onion Repo

## b283
*Mar 9, 2016*

Fix for failing build (caused by having some new options missing in the kernel config)

## b282
*Mar 7, 2016*

Updates to the Onion SPI library
* better handling of device registration
* usage printout is nicer

## b281
*Mar 5, 2016*

* Added NodeJS v4.3.1 as a package
  * Along with associated kernel changes
* Updates to Onion SPI library 
* arduino-dock, avrdude, and tmux packages are no longer included in the build

## b278
*Feb 23, 2016*

* Now building onion I2C python library as an optional module (in onion repo)
* Added Onion spi library, command line spi tool, and python module as modules in repo

## b277
*Feb 22, 2016*

I2C Expansions Software Updates
* Extended oled library functionality
  * functions for: text/image column addressing, writing a single byte to the screen, setting cursor by row and pixel
* Implemented Python object for the i2c library
* Updated all c-python modules with better error checking:
  * checks for correct arguments
  * checks for successful C function calls

## b275
*Jan 27, 2016*

Added `whois` command to Busybox

## b271
*Jan 25, 2016*

Updates to firmware database access scripts

## b270
*Jan 25, 2016*

Added `kmod-w1-slave-therm` kernel package for One-Wire temperature sensor family. Updated PATH variable to match new OpenWRT default.

## b269
*Jan 21, 2016*

Added `kmod-dma-buf` package to resolve build issue

## b267
*Jan 20, 2016*

Added one-wire kernel modules:
* `kmod-w1`
* `kmod-w1-gpio-custom`
* `kmod-w1-master-gpio`

## b266
Added gpio-test package to Onion Package Repo

## b265
Enabled GPIO Edge irq patch

Kernel Modules added:
* kmod-bluetooth 
* kmod-gpio-irq - enable interrupts from gpio

## b264
**Ando J1 Console Release**

Upgrading to version 0.0.6

Console Updates:
* Added Resistor Calculator App
* Implemented universal notification system
* Added error notification to I2C expansion apps if expansion cannot be found
* Cosmetic updates to Settings app, making it more user friendly
Firmware Update:
* ~~Added an additional patch for GPIO Edge irq~~
* Removed GPIO Edge irq patches

## b262
Added support for GPIO Edge irq

## b260
Added usb audio card kernel modules (`kmod-usb-audio` and `kmod-sound-core`)

## b259
oupgrade update:
* added ca-certificates packages to base build
* oupgrade script now points to https version of repo info file, various code clean-up
* onion sh lib: added function to wget a file from a url

## b258
* i2c library update: implemented writeBuffer function that takes an address and a data buffer, and then a private writeBuffer function that creates a buffer with the i2c register address at element 0, and the data in the rest of the elements, then uses this to perform the i2c write
* i2c-exp cli apps: changed verbosity option to be cumulative
* neopixel tool:
  * updated to use new i2c library writeBuffer function
  * fixed setBuffer class function: now returns valid EXIT_SUCCESS/EXIT_FAILURE

## b256, b255
python package update: added base packages to setup OmegaExpansion and OmegaArduinoDock Python Packages

## b254
neopixel-tool fix: set pixel can now accept hex codes with `0x` prefix

## b252
oled-exp fix: diagonal scrolling no longer leaves out the bottom row

## b251, b250, b249
Neopixel Class: 
* added brightness control to c++ class (added to cli app, c-lib, python module, and python example)
* added overloaded Init function that sets pin, length of strip, and performs init
* fixes for dynamic memory clean-up

## b247
Added utilities and libraries for controlling Neopixels with the Arduino Dock

## b245
oled-exp driver fix: resolved issue with column addressing for text and images, all images should display correctly now

## b244 - b241
**Invalid builds**
*resolving build server + strider deployment issues*

## b240, b239
Added i2c-exp python libraries as packages

## b238, b237, b236, b235
oled-exp changes
* fix for oledDraw: now reset the column addressing to 0-127 so images are displayed correctly
* changed name of contrasting setting function to oledSetBrightness
* fix: correctly set the cursor for columns (setcursor function now sets column addressing to 0-125 since changing the column addressing resets the column cursor pointer)

## b234
* Added g=grep alias to profile
* Updated pwm-exp usage printout to include oscillator sleep functionality

## b233
Console Bug Fix: permission denied error did not result in relogin

## b232
**Ando N2 Console Release**
Upgrading to version 0.0.5
Console Updates:
* Added apps for the PWM, Relay, and OLED Expansions
* Extended the GPIO Control App to include the Expansion Dock as well
* Added a taskbar to easily switch between apps

## b231
**Pre Console Release Test Build #2**
Fixes:
* Base system
  * Typo fix for ll alias
  * Onion Repo Key: now installs with correct name and as a signed signature file

## b230
**Pre Console Release Test Build**
Added functionality:
* Onion UBUS Service
  * added i2c-scan functionality: get a list of addresses of all connected I2C slaves
* I2C Expansions
  * pwm-exp
    * Implemented ability to set oscillator to sleep (disable all pwm signals)
    * Added check to see if oscillator is running, if not, start it up (automatic initialization, running with -i at the beginning is no longer necessary)
  * oled-exp
    * Developed driver for all oled exp functionality
      * init, clear, turn display on/off, invert display, dim the display, set the cursor, writing a message, scrolling (horizontal and diagonal), displaying an image
  * ubus integration for all three apps (pwm-exp, relay-exp, oled-exp)
  * created static libraries of the driver code for all three expansions (pwm, relay, oled)
* SH Library
  * json argument parser: added better parsing, now wraps values with double quotes if they have certain special characters
* Packages
  * Added Onion specific keys for signing Onion packages
  * Compiled NodeJs v0.10.5, available as a package in Onion Repo
* Base System
  * Will now preserve /root during a sysupgrade (firmware update)
  * Enabled access to the Onion Package Repo (opkg feed setup)
  * Added automatic ll alias
* Deploy
  * Sign Onion Repo packages with Onion key
  * Deploy all packages to the Onion Repo

## b229
Integration of i2c-exp-drivers into ubus via rpcd

## b228
Package repo keys feed now uses the keys from the Buildroot (this) Repo

## b227
Resolved conflict with OpenWRT NodeJS package, fixes for packages signature generation

## b225
Moved keys to buildroot repo. Changed scripts to push new signature of Packages file.

## b223 
Added nodejs package, deploy also pushes Onion packages to the repo

## b222
Added signature keys for Onion Package Repo

## b221
* Arduino Dock update: added placeholder lua sketch that is used by Arduino IDE when flashing
* i2c library: updated read function to return a buffer array, meant to increase compatibility as a library
* debug library: added an onion debug library, implements print statements with a severity level for now
* i2c-exp-drivers: updated to reflect i2c lib changes and debug lib addition
* ads1x15 driver: updated to reflect i2c lib changes and debug lib addition

## b220
Added arduino-dock package: all utilities required for use of Arduino Dock

## b219 
Console GPIO App fix

## b217
Added avrdude programmer and its' dependencies to build

## b216
Added kmod-hid, kmod-usb-hid, and kmod-usb-printer modules to build.

## b214
New console release.
Incrementing version number.

## b213
Added sftp server module

## b212
* updates to i2c lib, fixed issue with write/read function status return
* added driver for ads1x15 chip as built-in module
* added modules kmod-video-uvc and mjpg-streamer for use with USB webcams

## b211
* fast-gpio: implemented print verbosity control, along with json output support
* onion ubus: added fast-gpio support (pwm times-out)

## b209
* i2c library: headers and static library files are now properly installed and can be used by other packages
* added app to use ADS1X15 ADC+PGA chip

## b208
* i2c library: implemented output verbosity and some code clean-up
* sh lib: added clean-up of empty log files
* added chpasswd utility to busybox

## b207
i2c lib: expanded to perform multi-byte i2c read and write operations

## b206, b205 
relay-exp tool now supports different dip-switch settings to change I2C address, added better error checking

## b204
Changes to i2c expansions:
* changed the repo name
* clean-up of pwm-exp print outs
* exposed i2c and mcp23008 libraries

## b203, b202
Added login utility for busybox
Changed shellinabox to launch busybox login utility

## b199
Fix for shellinabox default PATH variable

## b198, b197
Added relay-exp application driver for the Relay expansion
Added i2c-tools as built-in module

## b196
Infra update: automatically updates the firmware db when a new image is released

## b193
New version of pwm-exp: 
* fix for setting pwm frequency
* command line arguments can now be float
* supports only doing pwm init from command line
* added input mode where pulse width and period can be used
* added error checking for i2c reads

## b192, b191
Added support for ext2/3/4 filesystems

## b190
New version of fast-gpio: now disables running pwm if set or set-direction commands are used

## b189, b188
Added Onion Console, removed setup wizard. Incremented to version 0.0.3

## b187
Removed Pwm Expansion test script

## b186, b185
Temporary: added test script for pwm expansion

## b184
Changed shellinabox default css

## b183, b182
Added Onion PWM Expansion Driver

## b181
Updated shellinabox daemon management again: enabled auto-start

## b180
Added i2c-tools package as a module

## b179, b178
Login banner and shellinabox changes
179: Updated shellinabox package to pull from Onion repo, fixed issue with build setup script
178: Changed login banner, changes to shellinabox daemon management (no longer auto-starts or auto-respawns)

## b177
Onion sh lib updates for use with the Console

## b176
Added GPS expansion usb drivers

## b175
Added several usb-serial drivers

## b174, b173, b172
Fixes for fast-gpio
* b174: added ability to set and read direction of pins

## b171
Added libugpio and gpioctl packages to the build

## b170
Following changes:
* oupgrade: added build number to text-based check
* wifisetup: added option to specify Omega's IP addr when setting up AP network
* added fast-gpio
* added onion-sh-lib, only used by fast-gpio for now

## b169
Added fix for opkg sources

## b168
Added omega-led service to onion-ubus
Enables ubus control of the LED built-in to the Omega

## b167, b166
Updates to oupgrade and onion-ubus
* oupgrade: JSON output update: Now outputs build numbers for device and repo, and build_mismatch boolean (if the device and repo are the same version, checks the build numbers)
* Added dir-list to onion ubus service: returns an array of all directories within a specified directory

## b165, b164, b163
Added shellinabox to the build. Shellinabox provides an AJAX interface to the command line shell.

## b159
Updated to Version 0.0.2

