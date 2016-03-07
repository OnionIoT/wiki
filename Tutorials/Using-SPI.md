# Using the GPIOs for SPI Communication

The Omega supports running the SPI protocol through the GPIOs, making it handy to communicate with an SPI-enabled peripherals. To implement SPI communication, the Omega has a C library, a Python module, and a command-line tool. This article will focus on the command line tool, `spi-tool`



[[_TOC_]]


[//]: # (The SPI Protocol)

# What is SPI?

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



[//]: # (The Command Line Tool)

# The Command Line Tool

The `spi-tool` command line utility allows the user to read and write single bytes from an SPI device. In order for an SPI device to be used, it must first be registered with the system. The `spi-tool` utility can perform that action as well; giving the SPI device a bus number and device ID. This bus number and device ID must then be used again when transferring data with the SPI device.



[//]: # (The Command Line Tool: Setup an SPI Device)

## Setup an SPI Device

Since the SPI signals are sent through the GPIOs, the Omega's Operating System needs to be told which signals correspond to which GPIOs along with other SPI parameters.

The `spi-tool` utility allows you to easily register an SPI device with the system:
```
spi-tool -b <bus number> -d <device ID> [options] setup

```

This device registration will persist until the Omega is powered down or rebooted.


**Arguments**

The `bus number` and `device ID` arguments are required every time. The values of these two parameters don't mean anything, they just serve as identifiers for the device.


**Options**

The following options are available, allowing you to change the SPI setup:
```
  --frequency <Hz>         Set max SPI frequency
  --delay <us>             Set delay after the last bit transfered before optionally deselecting the device before the next transfer.
  --bpw <number>           Set number of bits per word
  --sck <gpio>             Set GPIO for SPI SCK signal
  --mosi <gpio>            Set GPIO for SPI MOSI signal
  --miso <gpio>            Set GPIO for SPI MISO signal
  --cs <gpio>              Set GPIO for SPI CS signal
  --3wire                  SI/SO signals shared
  --no-cs                  No chip select signal
  --cs-high                Set chip select to active high
  --lsb                    Transmit Least Significant Bit first
```



**Defaults**

By default, if none of the options are changed, the following values will be used:

| Option      | Default Value                      |
|-------------|-------------------------------------|
| speed       | 100000 Hz (100 kHz)                 |
| delay       | 0                                   |
| bitsPerWord | 0 (Corresponds to 8 bits per  word) |
| mode        | SPI Mode 0                          |
| SCK GPIO    | GPIO6                               |
| MOSI GPIO   | GPIO18                              |
| MISO GPIO   | GPIO1                               |
| CS GPIO     | GPIO7                               |


**Examples**

Setup an SPI device with the default options to bus 0, with device ID 1:
```
spi-tool -b 0 -d 1 setup
```

Setup an SPI device, bus 1, device ID 2, with a max speed of 400 kHz and a GPIO13 selected for the SCK signals:
```
spi-tool -b 1 -d 2 --speed 400000 --sck 13 setup
```

Setup an SPI device, bus 2, device ID 3, with a max speed of 320 kHz, the CS signal active-high, and using a three-wire setup:
```
spi-tool -b 2 -d 3 --speed 320000 --cs-high --3wire setup
```



[//]: # (The Command Line Tool: Read a Byte)

## Read a Byte

Finally, interacting with the SPI device! To read a single byte from a registered SPI device:
```
spi-tool -b <bus number> -d <device ID> [options] read <address>
```

This command will print the byte read from the specified address on the SPI device.


**Arguments and Options**

The `bus number` and `device ID` need to correspond to the values used to register the device! Additionally, any options used in the registration of the device need to be repeated in this command.

The `address` argument indicates the address from which to read on the SPI device.


**Examples**

Read a byte from address 0x11 from device 1 on bus 0 (registered above):
```
root@Omega-ABCD:~# spi-tool -b 0 -d 1 read 0x11
> SPI Read from addr 0x11: 0x81
```

Read a byte from address 0x00 from device 2 on bus 1 (registered above):
```
root@Omega-ABCD:~# spi-tool -b 1 -d 2 --speed 400000 --sck 13 read 0x00
> SPI Read from addr 0x00: 0xf8
```

Read a byte from address 0xaf from device 3 on bus 2 (registered above):
```
root@Omega-ABCD:~# spi-tool -b 2 -d 3 --speed 320000 --cs-high --3wire read 0xaf
> SPI Read from addr 0xaf: 0xbe
```



[//]: # (The Command Line Tool: Write a Byte)

## Write a Byte

Along with reading, you can also use `spi-tool` to write to the SPI device:
```
spi-tool -b <bus number> -d <device ID> [options] write <address> <value>
```


**Arguments and Options**

The `bus number` and `device ID` need to correspond to the values used to register the device! Additionally, any options used in the registration of the device need to be repeated in this command.

The `address` and `value` arguments indicate the address on the SPI device to be written, and the value to write, respectively. 



**Examples**

Write 0x42 to address 0x12 to device 1 on bus 0 (registered above):
```
root@Omega-ABCD:~# spi-tool -b 0 -d 1 write 0x12 0x42
> SPI Write to addr 0x12: 0x42
```

Write 0xfa to address 0x09 on device 2 on bus 1 (registered above):
```
root@Omega-ABCD:~# spi-tool -b 1 -d 2 --speed 400000 --sck 13 write 0x09 0xfa
> SPI Write to addr 0x09: 0xfa
```

Write 0x01 to address 0xbf on device 3 on bus 2 (registered above):
```
root@Omega-ABCD:~# spi-tool -b 2 -d 3 --speed 320000 --cs-high --3wire write 0xbf 0x01
> SPI Write to addr 0xbf: 0x01
```




[//]: # (Moving Beyond the Command Line)

# Moving Beyond the Command Line

The `spi-tool` utility allows for some basic interaction with SPI devices using the command line. With interesting projects and use-cases, you will require additional interaction with the SPI device that might not be suited to the command line.

Well, you're in luck! There is an [Onion developed C library and Python module](../../Documentation/Libraries/SPI-Library) that gives you the flexibility to use SPI devices however you want!





