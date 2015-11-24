# Controlling RGB LED on the Expansion Dock

The Expansion Dock has an LED that is controlled by three GPIO pins:

* Pin 17 for the Red LED
* Pin 16 for the Green LED
* Pin 15 for the Blue LED

## Using the Console:

The console is under development and is coming soon! Stay tuned!
 
## Using the Command Line:

You can use the expled tool to specify the colour of the LED. It will use software PWM to drive the three LEDs.

Usage:

```
expled <6 digit hex value>
```

Where the first two digits represent the Red value, the next two represent the Green value, and the last two represent the blue value.

Example:

```
expled 0xf21133
```

This will set the Red to `0xf2`, the Green to `0x11`, and the Blue to `0x33`.

The LED should now be pink-ish.

Check out [this site](http://www.color-hex.com/color/f21133%20) for more info on hex colours.