# SPI Library

The Onion SPI Library, `libonionspi` is a static C library that provides functions to easily read from and write to devices communicating with the Omega over the GPIOs via the SPI protocol. The library can be used in C and C++ programs.

Also available is a Python module, called `onionSpi`, that implements an SPI object using functions from the C Library.


[[_TOC_]]


[//]: # (The SPI Protocol)

# The SPI Protocol

The Serial Peripheral Interface (SPI) is a four-wire synchronous communication protocol, largely used to connect microprocessors or microcontrollers to sensors, memory, and other peripherals. 

The four signals are:

| SPI Signal | Meaning                                                       |
|------------|---------------------------------------------------------------|
| SCK        | System Clock                                                  |
| MOSI       | Master Out, Slave In - Data sent from the Master to the Slave |
| MISO       | Master In, Slave Out - Data sent from the Slave to the Master |
| CS/SS      | Chip Select/Slave Select                                      |

The fact that it is a *synchronous* data bus means that one of the lines is a clock, used to synchronize the bits being sent on the data lines. 

The protocol is based on the Master-Slave architecture, so the Master will generate the System Clock and the Slave Select signals. In systems with multiple slaves, there will be multiple Slave Select signals.

For more details on SPI, check out the [Wikipedia article](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus).


[//]: # (The SPI Protocol: Linux and SPI)

## Linux and SPI

Linux systems usually control SPI devices through kernel drivers. However, it is also possible to generate the SPI protocol by bit-banging on connecte GPIOs. Bit-banging SPI is done through an adapter usually found at `/dev/spidevX.Y` where `X` is the device number and `Y` is the bus number.

The Omega does not have an SPI bit-banging adapter setup by default, but this can be done by using the `insmod` command to insert a module into the kernel. The `libonionspi` library takes care of this for you so you don't have to deal with the intricacies.



[//]: # (MAJOR HEADING)
[//]: # (The C Library)

# The C Library

The `libonionspi` C library is a series of functions that implement SPI communication through the Linux device interface. 


[//]: # (Programming Flow)

## Programming Flow

Any program using the `libonionspi` library must first initialize the `spiParams` structure since it is used by all of the other library functions. Then, the `spiParams` structure members should be modified to suit the requirements of the use case. For example, the desired bus number and device ID should be programmed.

Transactions will not work if the SPI adapter is not registered with the system, there are functions to check if an adapter is registered, and if not, to register the adapter. There is also a setup function to set  additional parameters on the adapter.

After the above is complete, the functions to transfer data using the SPI protocol can be used freely




[//]: # (Using the Library)

## Using the Library

**Header File**

To add the Onion SPI Library to your C/C++ program, include the header file in your C code:
``` c
#include <onion-spi.h>
```

**Library for Linker**

In your project's makefile, you will need to add the following static libraries to the linker command:
``` c
-loniondebug -lonionspi 
```

The static libraries are stored in `/usr/lib` on the Omega.



[//]: # (Example Code)

## Example

An example of how the `libonionspi` library is used can be found in the C implementation of the [command line SPI tool.](https://github.com/OnionIoT/spi-gpio-driver/blob/master/src/main-spi-tool.c)

Additional example code can be found in the sections below.



[//]: # (Return Values)

## Return Values

All functions follow the same pattern with return values:

If the function operation is successful, the return value will be `EXIT_SUCCESS` which is a macro defined as `0` in `cstdlib.h.`

If the function operation is not successful, the function will return `EXIT_FAILURE` which is defined as `1`. 

Any deviations from this rule will be specified below.



[//]: # (Parameter Structure)

## Parameter Structure

The `spiParams` structure is the central location that stores all of the data required by the library functions:
``` c
struct spiParams {
	// bus number and device id
	int 	busNum;
	int 	deviceId;

	int		speedInHz;		// system clock frequency
	int 	delayInUs;		// delay after last bit transferred before optionall deselecting the device before the next transfer
	int 	bitsPerWord;	// how many bits are in a transfered word

	int 	mode;			// SPI mode: can be 0 to 3 (0 is the most common)
	int 	modeBits;		// additional SPI setup

	// Omega GPIO definitions
	int 	sckGpio;
	int 	mosiGpio;
	int 	misoGpio;
	int 	csGpio;
};
```

### SPI Mode Bits

Additional SPI options can be setup on the interface by adding specific bits to the `modeBits` member of the structure. Macros exist to define which bits need to be set.

The following macros are available:
* `SPI_3WIRE`
  * Use three-wire implementation of SPI where the MOSI and MISO lines are shared on a single GPIO.
* `SPI_NO_CS`
  * Modify the protocol to not generate a Chip-Select line.
* `SPI_CS_HIGH`
  * Modify the protocol for an active-high Chip-Select line.
* `SPI_LSB_FIRST`
  * Modify the protocol to transmit bytes LSB to MSB.
* `SPI_LOOP`
  * Enable loopback mode where the slave is configured to transmit the received bytes. This can be achieved with a single master by wiring the MISO and MOSI lines together.

To enable any one of the options, perform a bitwise `or` operation with the `modeBits` structure member:
``` c
params.modeBits |= SPI_CS_HIGH;
params.modeBits |= SPI_LSB_FIRST;
```

Then run the `spiSetupDevice()` function to set the options in the adapter device. More information on this function in the sections below.



[//]: # (SUB-HEADING)
[//]: # (Setup Functions)

## Setup Functions

The following functions serve to initialize the `spiParams` structure and do any SPI adapter setup required to actually perform transfers.

[//]: # (Initialize the Parameter Structure)

### Initialize the Parameter Structure: `spiParamInit()`

There is a function to initialize the `spiParams` structure with acceptable default values:
``` c
void spiParamInit (struct spiParams *params);
```

The default settings:

| SPI Setting | Programmed Default                  |
|-------------|-------------------------------------|
| speedInHz   | 100000 Hz (100 kHz)                 |
| delayInUs   | 0                                   |
| bitsPerWord | 0 (Corresponds to 8 bits per  word) |
| mode        | SPI Mode 0                          |


Sets the SPI lines to the following GPIOs:

| SPI Signal | Omega Gpio |
|------------|------------|
| SCK        | 6          |
| MOSI       | 18         |
| MISO       | 1          |
| CS/SS      | 7          |


**Arguments**

The `params` argument should be the structure you want to initialize passed by reference.


[//]: # (Check if SPI Device is Mapped)

### Check if SPI Device is Mapped: `spiCheckDevice()`

Performs a check to see if an SPI device with the specified bus number and device ID is mapped:

``` c
int spiCheckDevice (int busNum, int devId, int printSeverity);
```

**Return Value**

If the SPI device adapter is found, the function returns `EXIT_SUCCESS`, a macro mapped to `0`

If the SPI device adapter is **not found**, the function returns `EXIT_SUCCESS`

**Arguments**

The `busNum` and `devId` specify the bus number and device ID of the adapter, respectively.

The print severity refers to the Onion Debug Library verbosity level. For now set to `ONION_SEVERITY_DEBUG_EXTRA` for no additional messages printed.
**More info on this to come.**


**Examples**

To check if a device on bus 1 with device ID 2 is registered, and print out a message based on the result:
``` c
int status;

status 	= spiCheckDevice(1, 2, ONION_SEVERITY_DEBUG_EXTRA);
if (status == EXIT_SUCCESS) {
	printf("SPI Device is mapped.\n");
}
else {
	printf("WARNING: SPI Device NOT mapped!\n");
}
```


[//]: # (Register SPI Device)

### Register SPI Device: `spiRegisterDevice()`

This function will register an SPI device with the bus number, device ID, and other SPI parameters as specified in the parameter structure:

``` c
int spiRegisterDevice (struct spiParams *params);
```

It will first check if a device with the specified bus number and device ID is already registered. If it is, it will just return `EXIT_SUCCESS`. 

If not, it will attempt to register the SPI device adapter by inserting an SPI-gpio module into the kernel. If this operation is successful, the function will return `EXIT_SUCCESS`, if not, `EXIT_FAILURE` is returned.


The function uses the following information to register the device:
* The bus number 
* The device ID
* SPI mode
* The SPI speed
* The GPIO for the SCK line
* The GPIO for the MOSI line
* The GPIO for the MISO line
* The GPIO for the CS line


**Example**

A short example showing how to register an SPI device adapter:
``` c
int 	status;
struct  spiParams	params;

// initialize the SPI parameters
spiParamInit(&params);

// set the desired bus number and device id
params.busNum 	= 0;
params.devId 	= 1;

// change the CS line to use GPIO23
params.csGpio 	= 23;

// register the device
status 	= spiRegisterDevice(&params);

```


[//]: # (Setup SPI Device)

### Setup SPI Device: `spiSetupDevice()`

This function will setup additional SPI parameters on the device adapter
``` c
int spiSetupDevice (struct spiParams *params);
```

It will setup the following SPI parameters:
* The maximum speed of the link
* The number of bits per word
* Additional mode information, see [section on additional mode information above]()
[//]: # (Lazar: add this link)



[//]: # (SUB-HEADING)
[//]: # (Communication Functions)

## Communication Functions

Once a device adapter is registered, the functions below can be used to read from and write data to the device via SPI.


[//]: # (Transfer Data Function)

### Transferring Data: `spiTransfer()`

This is the function that implements all data transfers using the SPI protocol:
``` c
int spiTransfer	(struct spiParams *params, uint8_t *txBuffer, uint8_t *rxBuffer, int bytes);
```

**Arguments**

The `params` spiParams structure holds all of the relevant SPI information.

The `txBuffer` argument will hold the data to be transferred via SPI.

The `rxBuffer` argument will be populated by data received during the SPI transfer.

And finally, the `bytes` argument indicates the number of bytes being transferred. Note that the txBuffer and rxBuffer need to be allocated to at least this size.


[//]: # (Transfer Data Function: Example Code: Reading)

#### Example Code: Reading a Byte

The following example code uses the SPI protocol to read a byte of data from a specific address on an SPI device:

``` c
void SpiReadValue(int addr)
{
	int 		status, size;
	uint8_t 	*txBuffer;
	uint8_t 	*rxBuffer;

	struct spiParams	params;
	
	// initialize the SPI parameters and set the bus number and device ID
	spiParamInit(&params);
	params.busNum		= 0;
	params.deviceId 	= 1;

	// set the transmission size and allocate memory for the buffers
	size 		= 1;
	txBuffer	= (uint8_t*)malloc(sizeof(uint8_t) * size);
	rxBuffer	= (uint8_t*)malloc(sizeof(uint8_t) * size);

	// assign the register address to the transmission buffer
	*txBuffer 	= (uint8_t)addr;

	// invoke the SPI transfer
	status 	= spiTransfer(&params, txBuffer, rxBuffer, size);

	// rxBuffer now contains the data read through the SPI interface
	printf("> SPI Read from addr 0x%02x: 0x%02x\n", addr, *rxBuffer);

	// clean-up
	free(txBuffer);
	free(rxBuffer);
}
```

During the SPI transfer, the SPI Master (the Omega) will send the contents of the transmission buffer, in this case, the address from which to read. The slave device will respond with a value that corresponds to the register address, this value will be populated into the receive buffer.

In this case, the `size` variable refers to the number of bytes to be read from the SPI device.

Before this function will work, the SPI device adapter needs to be registered, check out the section above. 

[//]: # (LAZAR: add link to section above)

Some devices require specific bit-wise operations on the address to indicate a read operation. Most common are:
* A bitwise shift to the left
* Setting a specific bit to 1 to indicate a read

Refer to the datasheet of your device for specifics.


[//]: # (Transfer Data Function: Example Code: Writing)

#### Example Code: Writing a Value

This example uses the SPI protocol to write a byte of data to a specific address on an SPI device:

``` c
void SpiWriteValue(int addr, int value)
{
	int 		status, size;
	uint8_t 	*txBuffer;
	uint8_t 	*rxBuffer;

	struct spiParams	params;

	// initialize the SPI parameters and set the bus number and device ID
	spiParamInit(&params);
	params.busNum		= 0;
	params.deviceId 	= 1;

	// set the transmission size and allocate memory for the buffers
	size 		= 2;
	txBuffer	= (uint8_t*)malloc(sizeof(uint8_t) * size);
	rxBuffer 	= (uint8_t*)malloc(sizeof(uint8_t) * size);

	// assign the register address and data to be written to the transmission buffer
	txBuffer[0] = (uint8_t)addr;
	txBuffer[1] = (uint8_t)value;

	// invoke the SPI transfer
	status 	= spiTransfer(&params, txBuffer, rxBuffer, size);

	// data has been written
	// any response is now in rxBuffer (usually don't get a response from a write operation so it should be empty)
	printf("> SPI Write to addr 0x%02x: 0x%02x\n", txBuffer[0], txBuffer[1] );

	// clean-up
	free(txBuffer);
	free(rxBuffer);
}
```

In this case, the SPI Master will first send the register address where the write is to be performed, and then the value to be written. Usually the SPI Slave will not send a response, however, the `spiTransfer()` function still requires a buffer to be passed in order to work properly.

[//]: # (Transfer Data Function: Example Code: Writing Stream of Data)

#### Example Code: Writing Several Values

Some devices allow/require the user to write a stream of data with no address, this can be accomplished using the `spiTransfer()` function as well:

``` c
void SpiWriteValues(int addr, int* data, int size)
{
	int 		status;
	uint8_t 	*txBuffer;
	uint8_t 	*rxBuffer;

	struct spiParams	params;

	// initialize the SPI parameters and set the bus number and device ID
	spiParamInit(&params);
	params.busNum		= 0;
	params.deviceId 	= 1;

	// allocate memory for the buffers based on input data size
	txBuffer	= (uint8_t*)malloc(sizeof(uint8_t) * size);
	rxBuffer 	= (uint8_t*)malloc(sizeof(uint8_t) * size);

	// fill the transmission buffer with the data
	for (i = 0; i < size; i++) {
		txBuffer[i] = (uint8_t)data[i];
	}

	// invoke the SPI transfer
	status 	= spiTransfer(&params, txBuffer, rxBuffer, size);

	// data has been written
	// any response is now in rxBuffer (usually don't get a response from a write operation so it should be empty)
	printf("> SPI: wrote a %d bytes of data to device\n", size );

	// clean-up
	free(txBuffer);
	free(rxBuffer);
}
```


[//]: # (Additional Functions)

### Additional Functions

The `spiTransfer()` is all that's required to perform reads and writes using SPI. However, two additional functions are provided to perform reads and writes. Internally, they both use the `spiTransfer()` function, but they provide a slightly simpler interface.


[//]: # (Additional Functions: spiRead)

#### Reading Data: `spiRead()`

This function can be used to perform a register read:
``` c
int spiRead(struct spiParams *params, int addr, uint8_t *rdBuffer, int bytes);
```

It can read a specified number of bytes from a specified address. The limitation is that the address can only be a single byte. Use `spiTransfer()` if your use-case requires more than 8-bits for the address.

**Arguments**

The `params` spiParams structure holds all of the relevant SPI information.

The `addr` argument is the 8-bit address from which to read.

The `rdBuffer` argument will be populated by data received during the SPI transfer.

And finally, the `bytes` argument indicates the number of bytes being read. Note that the `rdBuffer` needs to be allocated to at least this size. 


##### Example Code

The following function will read two bytes from the specified address, the data will be stored in `rdBuffer`:

``` c
void SpiReadValue2(int addr)
{
	int 		status, size;
	uint8_t 	*rdBuffer;

	struct spiParams	params;
	
	// initialize the SPI parameters and set the bus number and device ID
	spiParamInit(&params);
	params.busNum		= 0;
	params.deviceId 	= 1;

	// set the transmission size and allocate memory for the buffers
	size 		= 2;
	rdBuffer	= (uint8_t*)malloc(sizeof(uint8_t) * size);

	// invoke the SPI read
	status 		= spiRead(&params, addr, rdBuffer, size);

	// rdBuffer now contains the data read through the SPI interface
	printf("> SPI Read from addr 0x%02x: 0x%02x, 0x%02x\n", addr, rdBuffer[0], rdBuffer[1]);

	// clean-up
	free(rdBuffer);
}
```

In essence, this simplifies the use of the `spiTransfer()` for read scenarios.
*Note that this function has not been tested as thoroughly as the `spiTransfer()` function.*



[//]: # (Additional Functions: spiWrite)

#### Reading Data: `spiWrite()`

This function can be used to perform a register write:
``` c
int spiWrite(struct spiParams *params, int addr, uint8_t *wrBuffer, int bytes);
```

It will write a specified number of bytes to a specified address. The limitation is that the address can only be a single byte. Use `spiTransfer()` if your use-case requires more than 8-bits for the address.

**Arguments**

The `params` spiParams structure holds all of the relevant SPI information.

The `addr` argument is the 8-bit address on which to perform the write

The `wrBuffer` argument holds the data to be transmitted during the SPI transfer.

And finally, the `bytes` argument indicates the number of bytes being written. Note that the `wdBuffer` needs to be allocated to at least this size. 


##### Example Code

The following code will write a byte to the specified address, the data will be stored in `rdBuffer`:

``` c
void SpiWriteValue2(int addr, int value)
{
	int 		status, size;
	uint8_t 	*wrBuffer;

	struct spiParams	params;
	
	// initialize the SPI parameters and set the bus number and device ID
	spiParamInit(&params);
	params.busNum		= 0;
	params.deviceId 	= 1;

	// set the transmission size and allocate memory for the buffers
	size 		= 1;
	wrBuffer	= (uint8_t*)malloc(sizeof(uint8_t) * size);

	// put the value to be written in the wrBuffer
	*wrBuffer 	= (uint8_t)value;

	// invoke the SPI read
	status 		= spiWrite(&params, addr, wrBuffer, size);

	// data in wrBuffer has been written to the SPI device
	printf("> SPI Write to addr 0x%02x: 0x%02x\n", addr, *wrBuffer );

	// clean-up
	free(wrBuffer);
}
```

This function simplifies the use of `spiTransfer()` for scenarios where SPI is used to write to a register.
*Note that this function has not been tested as thoroughly as the `spiTransfer()` function.*







[//]: # (MAJOR HEADING)
[//]: # (The Python Module)

# The Python Module

The `onionSpi` Python module provides a Python object, `OnionSpi`, that serves as a wrapper around the C library functions. The usage is slightly different since the Python module is object oriented and the C library is just a set of functions.


[//]: # (Python: Programming Flow)

## Programming Flow

The Python module revolves around the `OnionSpi` object; once it is initialized with the bus number and device ID that the desired SPI adapter uses, the rest of the class functions can be used to register the adapater, setup the adapter's SPI options, and most importantly, make data transfers!

Once the object is initialized, the recommended flow is as follows:
* Check that the device adapter is registered
* If not, register the device adapter with the system
* Set any additional SPI parameters and setup the adapter with them
* Perform any SPI transfers as required.

The details of the specific functions to perform these actions are outlined below.


[//]: # (Using the Python Module)

## Using the Python Module

**Installing the Module**

To install the Python module, run the following commands:
```
opkg update
opkg install python-light pyOnionSpi
```

This will install the module to `/usr/lib/python2.7/OmegaExpansion/`

*Note: this only has to be done once.*


**Using the Module**

To add the Onion SPI Module to your Python program, include the following in your code:
``` python
import onionSpi
```

[//]: # (Python: Example Code)

## Example

An example of how the `OnionSpi` object is used can be found in the [`spi-gpio-driver` repo.](https://github.com/OnionIoT/spi-gpio-driver/blob/master/examples/testSpi.py)



[//]: # (Python: Initialization)

## Initialization of the Object

The object needs to be initialized before it can be used for reading and writing:

``` python
spi  = onionSpi.OnionSpi(busNumber, deviceId)
```

**Arguments**

The constructor requires the bus number and device ID of the SPI device adapter that is to be used. These numbers correspond to the device adapter itself: `/dev/spidevX.Y` where `X` is the device number and `Y` is the bus number.


[//]: # (Python: Object Member Variables)

## Object Member Variables

The `OnionSpi` object has several member variables that are used to define specific SPI parameters and settings. The can be accessed and modified directly using the object.

The member variables are as follows:

| Object Member | Meaning                                                                                                 |
|---------------|---------------------------------------------------------------------------------------------------------|
| `bus`         | The device adapter bus number                                                                           |
| `device`      | The device adapter device ID                                                                            |
| `speed`       | Maximum transmission clock speed in Hz                                                                  |
| `delay`       | Delay in us after last bit transferred before optionall deselecting the device before the next transfer |
| `bitsPerWord` | Number of bits in a transmitted word                                                                    |
| `mode`        | SPI mode: can be 0 to 3 (Mode 0 is the most common)                                                     |
| `modeBits`    | Additional SPI operating parameters                                                                     |
| `sck`         | GPIO for SPI SCK signal                                                                                 |
| `mosi`        | GPIO for SPI MOSI signal                                                                                |
| `miso`        | GPIO for SPI MISO signal                                                                                |
| `cs`          | GPIO for SPI CS signal                                                                                  |

The `modeBits` parameter may be a little difficult to work with so the following member variables were added: 

| Object Member | Meaning                                                         |
|---------------|-----------------------------------------------------------------|
| `threewire`   | Three-wire SPI: MOSI and MISO lines are shared on a single GPIO |
| `lsbfirst`    | Modify the protocol to transmit bytes LSB to MSB                |
| `loop`        | Enable loopback mode                                            |
| `noCs`        | Modify the protocol to not generate a Chip-Select line          |
| `csHigh`      | Modify the protocol for an active-high Chip-Select line         |

Changing any of these parameters will modify the `modeBits` member as well.


[//]: # (Python: Object Member Variables: Defaults)

### Default Values

When the object is first initialized, all of the member variables are initialized to legitimate values:

| SPI Setting   | Programmed Default                  |
|---------------|-------------------------------------|
| `speed`       | 100000 Hz (100 kHz)                 |
| `delay`       | 0                                   |
| `bitsPerWord` | 0 (Corresponds to 8 bits per  word) |
| `mode`        | SPI Mode 0                          |


Sets the SPI lines to the following GPIOs:

| SPI Signal | Omega Gpio |
|------------|------------|
| SCK        | 6          |
| MOSI       | 18         |
| MISO       | 1          |
| CS/SS      | 7          |


[//]: # (Python: Object Member Variables: Examples)

### Examples

Creating an SPI object, modifying some of the parameters, and then printing parameters:

``` python
spi 	= onionSpi.OnionSpi(0, 1)

spi.cs 		= 20
spi.sck		= 19
spi.csHigh 	= 1
spi.speed 	= 400000

print 'SPI CS GPIO:   %d'%(spi.cs)
print 'SPI SCK GPIO:  %d'%(spi.sck)
print 'SPI MISO GPIO: %d'%(spi.miso)
print 'SPI MOSI GPIO: %d'%(spi.mosi)

print 'SPI Speed: %d Hz (%d kHz)'%(spi.speed, spi.speed/1000)
print 'Mode Bits: 0x%x, CS-High: %d'%(spi.modeBits, spi.csHigh)
```



[//]: # (SUB-HEADING)
[//]: # (Python: Setup Functions)

## Setup Functions

The following functions are used to register the SPI device adapter and setup any SPI protocol options as required.


[//]: # (Python: Check if Device is Registered)

### Check if Device is Registered

To check if a device adapter with the bus number and device ID that were specified in the object constructor is already registered with the system:

``` python
return = spi.checkDevice()
```

**Return Values**

The function will return `0` if the device adapter is already mapped.

The return value will be `1` if the device adapter is NOT mapped/


[//]: # (Python: Register Device)

### Register Device

This function will register an SPI device with the bus number, device ID, and other SPI parameters as specified in the object variable members:

``` python
return = spi.registerDevice()
```

It will first check if a device with the specified bus number and device ID is already registered. If it is, it will just return `0`. 

If not, it will attempt to register the SPI device adapter by inserting an SPI-gpio module into the kernel. If this operation is successful, the function will return `0`, if not, `1` is returned.

The function uses the following information to register the device:
* The bus number 
* The device ID
* SPI mode
* The SPI speed
* The GPIO for the SCK line
* The GPIO for the MOSI line
* The GPIO for the MISO line
* The GPIO for the CS line


**Example**

A short example showing how to register an SPI device adapter:
``` python
spi 	= onionSpi.OnionSpi(0, 1)

# modify any SPI settings as required
spi.cs 		= 20
spi.speed 	= 400000

# register the device
spi.registerDevice()
```


[//]: # (Python: Setup SPI Device)

### Setup SPI Device

This function will setup additional SPI parameters on the device adapter
``` python
return  = spi.setupDevice()
```

It will setup the following SPI parameters:
* The maximum speed of the link
* The number of bits per word
* Additional mode information, see [section on additional mode information above]()
[//]: # (Lazar: add this link)


[//]: # (Python: Setup SPI Device)

### Conclusion

Once a device is registered, data can be read from and written to the device via SPI.


[//]: # (Python: Reading Functions)

## Reading Function

This function reads a specified number of bytes from a specified address on an SPI device:
``` python
values 	= spi.readBytes(addr, size)
```

The read bytes are returned in the form of a list, even if there is only one byte.


**Arguments**

The `addr` argument specifies from which address on the SPI device to read.

The `size` argument specifies the number of bytes to read.


**Examples**

Read a byte from address 0x33:
``` python
rdVal = spi.readBytes(0x33, 1)
# rdVal[0] now contains the byte that was read
```

[//]: # (LAZAR: double check this syntax^^)


Read three byres from address 0x00:
``` python
rdVal = spi.readBytes(0x00, 3)
# the rdVal list now contains the three bytes that were read 
```



[//]: # (SUB-HEADING)
[//]: # (Python: Writing Functions)

## Writing Functions

The `OnionSpi` class has two functions that can be used to write data via SPI:
* One that requires an address and a list of bytes to write
* One that just sends a list of bytes to write


[//]: # (Python: Write Bytes)

### Write Bytes

This function will write a list of bytes to a specified address on an SPI device
``` python 
return 	= spi.writeBytes(addr, values)
```

**Arguments**

The `addr` argument specifies to which address on the SPI device to write.

The `values` argument is a list of values to be written. Even if there is only a single byte, it should be in list form.

**Examples**

Write 0x23 to address 0x91:
``` python
ret 	= spi.writeBytes(0x91, [0x23])
```

Write 0x23, 0x33, 0x07 to address 0x96:
``` python
vals 	= [0x23, 0x33, 0x07]
ret 	= spi.writeBytes(0x96, vals)
```


[//]: # (Python: Write Bytes without an Address)

### Write Bytes without an Address

This function will just write a list of bytes to an SPI device (without specifying an address):
``` python
return 	= spi.write(values)
```


**Arguments**

The `values` argument is a list of values to be written. Even if there is only a single byte, it should be in list form.


**Examples**

Write 0x08, 0x34, 0x02, 0x07:
``` python
ret 	= spi.write([0x08, 0x34, 0x02, 0x07])
```

Write 0x00, 0x1C, 0x07, 0x12, 0x37, 0x32, 0x29, 0x2D: 
``` python
vals 	= [0x00, 0x1C, 0x07, 0x12, 0x37, 0x32, 0x29, 0x2D]
ret 	= spi.write(vals)
```

Write 0x11:
``` python
ret 	= spi.write([0x11])
```


