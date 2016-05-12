# How to Use GPIOs?

The Omega has 15 GPIO pins that can be controlled with software. 
The Expansion Dock allows for easy access to 12 GPIO pins and connects 3 pins to the LED on the Dock

![Omega GPIO](//i.imgur.com/5wN5KKe.png)

## Using the Command Line

We've developed a tool that makes GPIO access very easy using the command line, the tool is called fast-gpio

### Command Usage:

For a print-out of the usage, run fast-gpio with no comments on the command line:

```
root@Omega-0100:/# fast-gpio
Usage:
 fast-gpio set-input <gpio>
 fast-gpio set-output <gpio>
 fast-gpio get-direction <gpio>
 fast-gpio read <gpio>
 fast-gpio set <gpio> <value: 0 or 1>
 fast-gpio pwm <gpio> <freq in Hz> <duty cycle percentage>
```

### Setting a GPIO pin's direction:

```
fast-gpio set-input <gpio>
fast-gpio set-output <gpio>
```

A pin can be configured to either be input or output. 

To avoid damaging your Omega, set a pin to the input direction before driving any voltage to it!!

*(Note: this is available in firmware 0.0.2 b174 and later)*

### Reading a GPIO pin's direction:

```
fast-gpio get-direction <gpio>
```

Might be handy to check a pin's programmed direction

```
GPIO14 direction is OUTPUT
GPIO13 direction is INPUT
```

*(Note: this is available in firmware 0.0.2 b174 and later)*

### Reading a GPIO pin's value:

```
fast-gpio read <gpio pin>
```

This will return the pin's value, in both input and output modes

```
Read GPIO14: 0
```

### Setting a GPIO pin's value:
This will drive the selected pin to the value desired.

```
fast-gpio set <gpio pin number> <value to set; 0 or 1>
```

This will only work when the pin is in the output direction, but fast-gpio will take care of that behind the scenes.

### Using a pin as a digital input:

The pin needs to first be set to run as input

```
fast-gpio set-input 13
```

Then the connected voltage can be read:

```
fast-gpio read 13
Read GPIO13: 1
```

## Using the Console

The console app to control the GPIOs is under development and coming very soon! Stay tuned!