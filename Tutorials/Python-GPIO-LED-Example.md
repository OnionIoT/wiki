# Getting Started Example with the Onion Omega & Python

In this quick tutorial you'll learn how to control the in-built LED in the expansion dock using Python.

**Time required:** < 5 minutes

![Python Example](https://dl.dropboxusercontent.com/u/12816733/onion-omega-github-python-example-1.jpg "Python Example")

# Installation

First you'll need to install python & also git for this example. To do this run the following commands from the console

```
opkg update
opkg install python-light git git-http
```

This will take a few moments, once complete let's grab the example code from GitHub by running these commands:

```
cd /root
git clone https://github.com/BravoPapa/OmegaGPIO
```


# Running the Example

Excellent, let's now change directories and run the example python script.

```
cd OmegaGPIO 
python gpio_demo.py
```

The RGB LED on your expansion dock will now be rotating through red, green & blue.

Neat eh?


## Using the GPIO Functions

Take a look at `omega_gpio.py` file to see the available functions that can be used in your own Python projects.

### Using a GPIO as Output

Here is a small sample of code that sets GPIO13 to HIGH and then LOW 5 seconds later:
``` python
import omega_gpio
import time

# initialize the pin
gpioNum = 13
omega_gpio.initpin(gpioNum,'out')

# set the pin to HIGH
omega_gpio.setoutput(gpioNum, 1)

# wait 5 seconds and set to LOW
time.sleep(5)
omega_gpio.setoutput(gpioNum, 0)

# release the pin
omega_gpio.closepin(gpioNum,'out')
```


### Using a GPIO as Input

This sample of code shows how to read and print the input value of a GPIO every 5 seconds for a minute:
``` python
import omega_gpio
import time

# initialize the pin
gpioNum = 26
omega_gpio.initpin(gpioNum,'in')

# perform 12 reads, each 5 seconds apart
for i in range (0, 12):
	print 'GPIO%d value: %d'%(gpioNum, omega_gpio.readinput(gpioNum) )
	time.sleep(5)

# release the pin
omega_gpio.closepin(gpioNum,'out')
```




# Further Reading

A huge hat-tip to [Brian Piersel](https://community.onion.io/user/brian-piersel) for posting this example code in the [community forums](https://community.onion.io/topic/502/gpio-and-python/3).

And a thank you from Onion to [Matthew Ogborne](https://github.com/moggiex) for writing this tutorial. 

More details on the Onion Omega GPIO Pins can be [found here.](https://wiki.onion.io/Tutorials/Using-the-GPIOs)


