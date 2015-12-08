# Relay Expansion Library

The Onion Relay Expansion library, `libonionrelayexp` is a static C library that provies functions to setup and change the state of the relays on the Relay Expansion. 

[//]: # (LAZAR: Add image of relay-exp here)

The library can be used in C program for now, C++ support is coming soon.


[[_TOC_]]



[//]: # (Programming Flow)

## Programming Flow

After each power-cycle, the chip that controls the Relay Expansion must be programmed with an initialization sequence. After the initialization, the relays can be turned on and off at will.




[//]: # (Using the Library)

## Using the Library

**Header File**

To add the Onion I2C Library to your program, include the header file in your code:
``` c
#include <relay-exp.h>
```

**Library for Linker**

In your project's makefile, you will need to add the following static libraries to the linker command:
``` c
-loniondebug -lonioni2c -lonionmcp23008 -lonionrelayexp
```

The static libraries are stored in `/usr/lib` on the Omega.



[//]: # (I2C Device Address)

## I2C Device Address

The Relay Expansion is the only expansion that has a configurable I2C device address. This was done so that up to eight Relay Expansions can be stacked on a single Omega, giving the user the ability to control 16 relay modules independently.

The base device address is `0x20`, the dip switches control the offset added to the base address:
* The 'Off' position for each switch is when the toggle is close to the numbers on the switch, or away from the relay modules.
* The 'On' position is when the toggle is away from the numbers on the switch, or closer to the relay modules.

The table below defines the relationship:

| Switch 1 | Switch 2 | Switch 3 | I2C Device Address |
|----------|----------|----------|--------------------|
| Off      | Off      | Off      | *0x27*             |
| Off      | Off      | On       | *0x26*             |
| Off      | On       | Off      | *0x25*             |
| Off      | On       | On       | *0x24*             |
| On       | Off      | Off      | *0x23*             |
| On       | Off      | On       | *0x22*             |
| On       | On       | Off      | *0x21*             |
| On       | On       | On       | *0x20*             |


**All of the functions in this library will require an address argument that specifies the offset to add to the base address of `0x20`**



[//]: # (Functions)

## Functions

Each of the main functions implemented in this library are described below.



### Return Values

All functions follow the same pattern with return values:

If the function operation is successful, the return value will be `EXIT_SUCCESS` which is a macro defined as `0` in `cstdlib.h.`

If the function operation is not successful, the function will return `EXIT_FAILURE` which is defined as `1`. 

A few reasons why the function might not be successful:
* The specified device address cannot be found on the I2C bus
* The system I2C interface is currently in use elsewhere

An error message will be printed that will give more information on the reason behind the failure.


### Types

This library has only one enumerated type defined and it is meant to easily define which relay module on the device is to be used.

The enumerated type is defined as follows:
``` c
typedef enum e_RelayDriverChannels {
	RELAY_EXP_CHANNEL0 		= 0,	
	RELAY_EXP_CHANNEL1,
	RELAY_EXP_NUM_CHANNELS,
} ePwmDriverAddr;
```

![Relay Channel Identification](https://i.imgur.com/Wk6Z9lW.png)



[//]: # (Init Function)

### Initialization Function

This function programs the initialization sequence on the Relay Expansion, after this step is completed, the functions to set the relay states can be used with success:
``` c
int relayDriverInit (int addr);
```

**Arguments**

The `addr` argument is described above in the [I2C Device Address section](#i2c-device-address).


**Examples**

Initialize a Relay Expansion with all switches set to 0, meaning the I2C device address will be 0x27:
``` c
int status 	= relayDriverInit(7);
```

Initialize with switches set to on-off-on (device address: 0x22):
``` c
int status 	= relayDriverInit(2);
```

Initialize with switches set to on-on-off (device address: 0x24):
``` c
int status 	= relayDriverInit(4);
```


[//]: # (Check Init Function)

### Check for Initialization 

This function performs several reads to determine if the Relay Expansion requires the initialization sequence to be programmed before the relay states can be changed.

``` c
int relayCheckInit (int addr, int *bInitialized);
```

**Arguments**

The `addr` argument is described above in the [I2C Device Address section](#i2c-device-address).

The `bInitialized` argument follows the table below:

| Initialization Status | bInitialized |
|-----------------------|--------------|
| Not Initialized       | 0            |
| Initialized           | 1            |


**Example**

Check if a Relay Expansion (with all switches set to On) is initialized:
```c
int status, bInit;
status 	= relayCheckInit(0, &bInit);

if (bInit == 0) {
	printf("The Relay Expansion needs to be initialized\n");
}
else {
	printf("The Relay Expansion is good to go!\n");
}
```



[//]: # (Set Relay State Function)

### Set Relay State

Finally the fun stuff! Use this function to change the state of the relay:

``` c
int relaySetChannel	(int addr, int channel, int state);
```

**Arguments**

The `addr` argument is described above in the [I2C Device Address section](#i2c-device-address).

The `channel` argument selects the relay in question. See the [diagram above](#functions_types) for info on which channel corresponds to which relay.

The `state` argument allows the user to select if the relay will be turned on or off:
* 0 turn the relay OFF
* 1 turn the relay ON



[//]: # (Set State for Both Relays Function)

### Set State for both Relays

In the event that both relays need to be turned on or off at the same time:

``` c
int relaySetAllChannels	(int addr, int state);
```

This is performed with a single register write so both relays should react at the same time.


**Arguments**

The `addr` argument is described above in the [I2C Device Address section](#i2c-device-address).

The `state` argument allows the user to select if the relays will be turned on or off:
* 0 turn the relays OFF
* 1 turn the relays ON




