# Using the Power Dock

Power your next project wirelessly using the Power Dock. Equipped with on-board recharging circuity, the Power Dock allows you to recharge and monitor battery levels. 

[[_TOC_]]

## Differences from Expansion Dock

We would like to point out main differences between Expansion Dock and Power Dock

**Image Highlighting Differences**

### No USB to Serial Chip

There is not USB to Serial Chip on the Power Dock. This means that you will **not** be able to connect to the Omega serially using the microUSB port.

### The Power Switch

On the Expansion Dock, the power switch turns the power on or off from the microUSB cable. 

On the Power Dock, the power switch controls power to the Omega, regardless of whether it is powered from the battery or microUSB cable. **Note:** The power switch has no effect on the battery charging, i.e The battery will charge regardless of the switch position. 

### Indicator LEDs

The power dock contains 4 LEDs that indicate the current battery level and charging status. The LED closest to the microUSB port indicates the lowest battery level and the LED furthest away from the microUSB port indicates the highes battery level. 

## Hardware Specs

Below are the relevant hardware specs for the Power Dock.

### Battery

Specifications:

* Type : LiPo
* Voltage: 3.7V
* Rating: Something
* Approx LifeTime: 
* Approx ChargeTime: 

### LED Indicators



## Usage Modes

The Power Dock supports 3 different usage modes

### Discharing Mode

This mode runs when the Omega is running of the battery only. The LED Indicators will be turned off by default to conserve power, however they can be turned on via an Omega command. 

### Charging Mode

This mode runs when the battery and microUSB cable is connected to the Omega. The battery will charge in this mode regardless of the power switches position. The bottom Indicator LED will also be blinking, indicating that the battery is being charged. **Note:** For best results use a short microUSB cable for charging.

### Expansion Mode

This mode runs when the battery is disconnected and the Omega is receiving power from the microUSB cable. **Note:** The Indicator LEDs maybe flashing irattically, observe the gif below.

## Power Dock Application

###Install

To install the power-dock application, run the following commands:

```
opkg update
```

```
opkg install power-dock
```

### Usage

The power-dock application allows you check the battery level by turning on the LED Indicators. To do this simply enter the following command:

```
power-dock
``` 

For help with the application, enter the following command:

```
power-dock -h
```