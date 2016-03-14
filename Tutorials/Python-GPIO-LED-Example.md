# Getting Started Example with the Onion Omega & Python

In this quick tutorial you'll learn how to control the in-built LED in the expansion dock using Python.

**Time required:** < 5 minutes

![Python Example](https://dl.dropboxusercontent.com/u/12816733/onion-omega-github-python-example-1.jpg "Python Example")

# Installing Python

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

# Further Reading

A huge hat-tip to [Brian Piersel](https://community.onion.io/user/brian-piersel) for posting this example code in the [community forums](https://community.onion.io/topic/502/gpio-and-python/3).

More details on the Onion Omega GPIO Pins can be [found here](https://wiki.onion.io/Tutorials/Using-the-GPIOs)


