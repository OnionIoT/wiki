# Reading 1-Wire Sensor Data with the Omega

*This tutorial is on how to set up the 1-Wire interface, connect sensors to GPIO pins and read sensor data with the Omega.*

The Omega can read sensor data via 1-Wire bus system connected to GPIO pins.

## Connecting Hardware

1-Wire sensors provide 3 connectors:

* Power Supply (3.3 V)
* GND (ground)
* DQ (data line)

Read your sensors data sheet to identify the appropriate pins!

Connect the sensors:

![Sensor Wiring Diagram](http://i.imgur.com/SIlLkpN.png)

|Sensor        |Onion Omega|Resistor 4.7 kOhm|
|--------------|-----------|-----------------|
|GND           |GND        |                 |
|PowerSupply   | 3.3 V     |pin 1            |
|DQ (data line)| GPIO #    |pin 2            |

* Where # can be any of the available GPIO pins [0, 1, 6, 7, 8, 12 ,13, 14, 18, 19, 23, 26]
* For the sake of this tutorial use GPIO 19
* The dimension of the pullup resistor may defer depending on:
  + the actual sensor (some may have built-in resistors)
  + the number of sensors connected
  + the length of the connection wire


## Setting up 1-Wire Bus System

Steps to setup 1-Wire bus system:

1. Ensure you are on Omega firmware `b270` or higher
2. Configure the GPIO pin connected to the data line of the sensor
	* `echo "w1-gpio-custom bus0=0,19,0" > /etc/modules.d/55-w1-gpio-custom`
	  * *replace 19 with the GPIO pin # you want to use*
3. `reboot` your Omega
	* Check if the following folder exists:
		* `/sys/devices/w1_bus_master1/28-000123456789/`
	    * Where *28-000123456789* is a unique id of your sensor and may look similar but not equally

## Read the Sensor Data

 1. Read raw sensor data
    * `cat  /sys/devices/w1_bus_master1/28-000123456789/w1_slave`
    * prints something similar for a temperature sensor [DS18B20]
```
	63 01 4b 46 7f ff 0c 10 d1 : crc=d1 YES
  63 01 4b 46 7f ff 0c 10 d1 t=22187
```
 2. Trimmed and formatted (for DS18B20)
    * `awk -F= '/t=/ {printf "%.03f\n", $2/1000}' /sys/devices/w1_bus_master1/28-000123456789/w1_slave`
	* would return
```
22.187
```
