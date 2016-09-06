# Installing Python

The Omega supports Python, we recommend installing the light version to save on space.

[[_TOC_]]


# Steps to Install

Connect to the Omega's terminal either using SSH or Serial.

Run the following commands on the terminal:
```
opkg update
opkg install python-light
```

This will take about 2MB of space on the Omega

###Python3
```
opkg update
opkg install python3-light
```

Then you have to run the right executable (python3). You can do a symbolic link to python so you just have to type python in the command line to run the interpreter.

## Full Installation

The full installation of Python supports more modules out of the box but takes up more space on the Omega. 

To install the full version:
```
opkg update
opkg install python
```
###Python3
```
opkg update
opkg install python3
```
Then you have to run the right executable (python3). You can do a symbolic link to python so you just have to type python in the command line to run the interpreter.
