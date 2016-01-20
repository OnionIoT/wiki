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
Added usb audio card drivers

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

