# PWM Expansion Library

The Onion Servo (PWM) Expansion library, `libonionpwmexp` is a static C library that provides functions to setup the Servo Expansion and generate PWM signals.

![PWM Expansion Photo](http://i.imgur.com/aNoYCZc.png)

The library can be used in C programs for now, C++ support is coming soon.

This library is also available as a module for use in Python. The module is called `pwmExp` and is part of the `OmegaExpansion` package.


[[_TOC_]]



[//]: # (Programming Flow)

## Programming Flow

After each power-cycle, the chip that controls the PWM Expansion must be programmed with an initialization sequence to enable the on-board oscillator so that PWM signals can be generated. 

After the initialization, the other functions can be used to generate PWM signals on specific channels or to change the PWM signal frequency.

Additionally, it is possible to disable to the oscillator, disabling the generation of all PWM signals at once. Before generating new PWM signals, the initialization sequence must be run again.



[//]: # (PWM Signal Refresher)

## PWM Signal Refresher

Pulse Width Modulated signals can be described with duty cycle percentages and frequencies/periods:

![Duty Cycle Graph](http://www.bristolwatch.com/picaxe/images/io43.gif)

The **duty cycle** indicates what percentage of time a signal is on or high voltage.

The **frequency** determines the overall period of the pulse.

For a more detailed explanation, see the guide on [using the Servo Expansion.](../Expansions/Using-the-Servo-Expansion#pwm-signals)



[//]: # (Using the C Library)

## Using the C Library

**Header File**

To add the Onion PWM Expansion Library to your program, include the header file in your code:
``` c
#include <pwm-exp.h>
```

**Library for Linker**

In your project's makefile, you will need to add the following static libraries to the linker command:
``` c
-loniondebug -lonioni2c -lonionpwmexp
```

The static libraries are stored in `/usr/lib` on the Omega.




[//]: # (Using the Python Module)

## Using the Python Module

**Installing the Module**

To install the Python module, run the following commands:
```
opkg update
opkg install python-light pyPwmExp
```

This will install the module to `/usr/lib/python2.7/OmegaExpansion/`

*Note: this only has to be done once.*


**Using the Module**

To add the Onion PWM Expansion Module to your Python program, include the following in your code:
``` python
from OmegaExpansion import pwmExp
```

The functions are largely the same as their C counterparts, including the arguments and return values. Any differences from the C library will be explicitly mentioned below. 




[//]: # (Functions)

## Functions

Each of the main functions implemented in this library are described below.



### Return Values

All functions follow the same pattern with return values:

If the function operation is successful, the return value will be `EXIT_SUCCESS` which is a macro defined as `0` in `cstdlib.h.`

If the function operation is not successful, the function will return `EXIT_FAILURE` which is defined as `1`. 

A few reasons why the function might not be successful:
* The specified device address cannot be found on the I2C bus (the Expansion is not plugged in)
* The system I2C interface is currently in use elsewhere

An error message will be printed that will give more information on the reason behind the failure.


### Channels

The PWM Expansion has 16 channels that can generate distinct PWM signals. Note that they will all be running on the same frequency.



[//]: # (Init Function)

### Initialization Function

This function programs the initialization sequence on the Servo Expansion, after this step is completed, the functions to generate PWM signals or change the signal frequency can be used with success:
``` c
int	pwmDriverInit ();
```

The oscillator will be setup to generate PWM signals at 50 Hz, the default for most servos.


**Examples**

Initialize the PWM Expansion:
``` c
int status 	= pwmDriverInit();
```


[//]: # (Python: Init Function)

#### Python Equivalent

In Python, this function performs the same operation and returns the same status. The Python function is:
``` python
pwmExp.driverInit()
```

**Examples**

Initialize the PWM Expansion:
``` c
status 	= pwmExp.driverInit();
```



[//]: # (Check Init Function)

### Check for Initialization 

This function performs several reads to determine if the Servo Expansion has been initialized and the oscillator is running.

``` c
int pwmCheckInit (int *bInitialized);
```

It can be used to check if calling the Initialization function is required.

**Arguments**

The `bInitialized` argument is to be passed by reference and once the function executes, it will contain a value that corresponds whether or not the Expansion is currently in the initialized state.
The value follows the table below:

| Initialization Status | bInitialized |
|-----------------------|--------------|
| Not Initialized       | 0            |
| Initialized           | 1            |


**Example**

Check if a Servo Expansion is initialized:
```c
int status, bInit;
status 	= pwmCheckInit(&bInit);

if (bInit == 0) {
	printf("The Servo Expansion needs to be initialized\n");
}
else {
	printf("The Servo Expansion is up and running!\n");
}
```


[//]: # (Python: Check Init Function)

#### Python Equivalent

In Python, this function performs the same operation, **however, the return value is not the status, it returns the Initialization Status referenced above**
``` python
pwmExp.checkInit()
```

**Example**

Check if the PWM Expansion is initialized:
``` python
bInit 	= pwmExp.checkInit()

if (bInit == 0):
	print 'The Servo Expansion needs to be initialized\n'
else:
	print 'The Servo Expansion is up and running!''
}
```




[//]: # (Generate PWM Signal Function)

### Generate a PWM Signal

Here we go! Use this function to generate a PWM signal on a specified channel:

``` c
int pwmSetupDriver (int driverNum, float duty, float delay);
```

**Arguments**

The `driverNum` argument is detemines on which channel to generate the PWM signal. The accepted values are:

| Value   | Meaning                                   |
|---------|-------------------------------------------|
| 0 to 15 | Matches the label on the Servo Expansion  |
| -1      | Generates the same signal on all channels |


The `duty` argument specifies duty cycle percentage for the PWM signal. Accepted values are **0 to 100**. Decimal values are allowed.


The `delay` argument specifies the percentage delay before the PWM signal goes high. Accepted values are **0 to 100** with decimal values being allowed. *In normal use with servos this should be set to 0.*


**Examples**

Set channel 0 to a PWM signal with a 50% duty cycle:
``` c
int status;
status = pwmSetupDriver(0, 50, 0.0f);
```

Generate a 3.55% duty cycle PWM signal with a 45% delay on channel 7: 
``` c
int status;
status = pwmSetupDriver(7, 3.55f, 45);
```

Set channel 15 to always on:
``` c
int status;
status = pwmSetupDriver(15, 100, 0);
```

Set channel 8 to always off:
``` c
int status;
status = pwmSetupDriver(8, 0, 0);
```

Set all channels to a 15.65% duty cycle PWM signal:
``` c
int status;
status = pwmSetupDriver(-1, 15.65f, 0.0f);
```


[//]: # (Python: Generate PWM Signal Function)

#### Python Equivalent

In Python, this function takes the same arguments, performs the same operation, and returns the same status indicator
``` python
pwmExp.setupDriver(channel, duty, delay)
```

**Example**

Set channel 7 to 6.55% duty cycle:
``` python
status = pwmExp.setupDriver(7, 6.55, 0)
```

Set all channels to 66.66% duty with a 9% delay:
``` python
status = pwmExp.setupDriver(-1, 66.66, 9)
```



[//]: # (Set Signal Frequency)

### Set PWM Signal Frequency

The oscillator can be reprogrammed to generate a variety of different frequencies:

``` c
int pwmSetFrequency	(float freq);
```

This will change the frequency of the PWM signals generated on all of the channels.
The oscillator can generate frequencies between **24 Hz and 1526 Hz,** the default value is **50 Hz.**


**Arguments**

The `freq` argument is a floating point number that specifies the frequency. The function will accept any input but the programmed frequency will be clamped between 24 Hz and 1526 Hz.


**Examples**

Change the frequency to 60 Hz and generate a 40% duty cycle signal on channel 14:
``` c
int status 	= pwmSetFrequency	(60.0f);
status 		= pwmSetupDriver	(14, 40, 0);
```

Generate a signal on channel 13, change the frequency to 105.45 Hz, and generate a new signal on channel 13:
``` c
int status 	= pwmSetupDriver	(13, 99, 0);
status 		= pwmSetFrequency	(105.45f);
status 		= pwmSetupDriver	(13, 82, 0);
```


[//]: # (Python: Set Signal Frequency)

#### Python Equivalent

In Python, this function takes the same argument, performs the same operation, and returns the same status indicator
``` python
pwmExp.setFrequency(frequency)
```

**Example**

Change the frequency to 60 Hz:
``` python
status = pwmExp.setFrequency(60)
```

Change the frequency to 92.23 Hz:
``` python
status = pwmExp.setFrequency(92.23)
```




[//]: # (Disable Oscillator)

### Disabling the Oscillator

The oscillator can also be disabled, automatically stopping all PWM signal generation:

``` c
int pwmDisableChip ();
```

This might be useful for disabling PWM signal-driven devices while not powering off the Omega.
**The initialization function will have to be run before new PWM signals can be generated.**


[//]: # (Python: Disable Oscillator)

#### Python Equivalent

This function is largely the same in Python:
``` python
pwmExp.disableChip(frequency)
```

**Example**

Disable the oscillator:
``` python
status 	= pwmExp.disableChip()
```


