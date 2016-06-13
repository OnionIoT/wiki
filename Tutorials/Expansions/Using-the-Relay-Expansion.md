# Using the Relay Expansion

The Relay Expansion allows you to control two relay modules. Relays are basically electronically operated switches, enabling the Omega's low power circuits to control other, potentially high power circuits.


[[_TOC_]]



[//]: # (Using Relays)

# Using Relays

Since relays are essentially switches, they can be used as switches in other circuits. 

Consider this LED circuit:

![LED Circuit Diagram](http://i.imgur.com/XjWmfh8.png)

We can build this circuit without the switch:

![LED Circuit](http://i.imgur.com/a0aH2lf.jpg)


Next, we will add the Omega and Relay Expansion to act as the switch:

![Omega + Relay Exp + LED circuit](http://i.imgur.com/i3VvWGu.jpg)

The positive lead of the battery pack is connected to the port labelled `IN` on the Expansion. A jumper wire is connected from the `OUT` port back to the circuit. Since the Relay is OFF, the switch is off and no current is passing through the LED:


![Relay Exp connection close-up](http://i.imgur.com/iPswHOC.jpg)


When we turn the relay on, it acts as a closed switch, allowing current to flow through the LED:

![Omega + Relay Exp + LED circuit on](http://i.imgur.com/hGa4uts.jpg)


The Relay Expansion has two modules, so it is possible to control two different circuits with a single Relay Expansion.



[//]: # (Hardware)

# The Hardware

The Relay Expansion has two TE Axicom IM03 relay modules. The table below outlines the specifications:

| Parameter                 | Specification             |
|---------------------------|---------------------------|
| Maximum Switching Voltage | 220VDC, 250VAC            |
| Current Rating            | 2A                        |
| Switching Power           | 60W, 62.5VA               |
| Switching Time            | Typical: 1ms Maximum: 3ms |

The full datasheet for the relay can be found [here](http://www.te.com/commerce/DocumentDelivery/DDEController?Action=showdoc&DocId=Specification+Or+Standard%7F108-98001%7FV%7Fpdf%7FEnglish%7FENG_SS_108-98001_V_IM_0614_v1.pdf%7F4-1462039-1).

## The Dip-Switches

The dip switches on the Relay Expansion control the I2C device address of the Expansion, enabling users to stack and control up to eight Relay Expansions with one Omega and Expansion dock.

The 'off' position for each switch is when the toggle is close to the numbers on the switch, or away from the relay modules.

The 'on' position is when the toggle is away from the numbers on the switch, or closer to the relay modules.



[//]: # (Using relay-exp)

# Using the Command Line

*Make sure that your Omega has the latest firmware!*

A tool we've developed called `relay-exp` will make controlling your relay expansion easy as pie.

Also available are a C library and a Python module that allow you to develop your own programs to control the Relay Expansion.


[//]: # (Using relay-exp: command usage)

## Command Usage

For a print out of the command's usage, run it with just a `-h` argument:

```
relay-exp -h
```


[//]: # (Using relay-exp: init)

## Initialization

After every power-cycle, the chip on the Relay Expansion must be initialized to correctly and safely control the Relay Modules. **The driver application will automatically detect if initialization is required and perform the required sequence,** so there is no need to run this command.

However, manually triggering the initialization is still available; run the following command:

```
relay-exp -i
```

This can be run on it's own or in conjunction with any commands below.

By default, the relays will be OFF.


[//]: # (Using relay-exp: relay channels)

## Relay Channels

The Expansion has two modules, this guide will refer to the relays as RELAY0 and RELAY1 or as a channel.

Refer to the following image to identify which channel refers to which relay:

![Relay Expansion](//i.imgur.com/Wk6Z9lW.png)


[//]: # (Using relay-exp: setting a relay's state)

## Setting a Relay's State

The tool allows you to program the relay's states like so:

```
relay-exp <channel> <state>
```

The channel argument should be either 0 or 1 for RELAY0 and RELAY1 respectively.

The state argument should be 0 or off for turning the relay OFF, or 1 or on for turning the relay ON

When the relay is OFF, it will act as an open switch, so any circuit connected to it will not be closed and will therefore be powered down. When it is ON, it is essentially a closed switch, so connected circuits will be powered on.

**A few examples...**

To initialize the chip and enable relay1:

```
relay-exp -i 1 1
```

To disable relay1:

```
relay-exp 1 0
```

To turn on relay 0:

```
relay-exp 0 on
```


[//]: # (Using relay-exp: controlling both relays simultaneously)

## Controlling both Relays Simultaneously

You are also able to control both relays in a single command:

```
relay-exp all <state>
```

As above, the state argument should be 0 or off to turn the relay OFF, or 1 or on to turn the relay ON, the only difference is that it will now affect both relays.

**Some Examples:**

Initializing the chip and turning both relays ON:

```
relay-exp -i all 1
```

Turning both relays OFF:

```
relay-exp all off
```


[//]: # (Using relay-exp: reading a relay's state)

## Reading a Relay's State

The tool also allows you to check the current state of the relay:

```
relay-exp read <channel>
```

The channel argument should be either 0 or 1 for RELAY0 and RELAY1 respectively.

The output of the program will indicate if the relay is ON or OFF.
	
**A few examples...**

To read the state of relay0:
```
root@Omega-1302:~# relay-exp read 0
> Reading RELAY0 state: ON
```

To read the state of relay1:
```
root@Omega-1302:~# relay-exp read 1
> Reading RELAY1 state: OFF
```



[//]: # (Switch Explanation)

# Using Multiple Relay Expansions by Changing the Dip-Switch Settings

The dip-switches specify the I2C address the chip on the Relax Expansion declares as it's device address. A single Omega and Expansion Dock can control up to eight Relay Expansions if they all have different dip-switch configurations.

The relay-exp tool will need to know if the switch configuration has changed when programming the expansion:

```
relay-exp -s <bbb> <channel> <state>
```

Where `<bbb>` is a binary number representing the position of each switch. 

If the switch is 'off', it can be represented with a 0; if the switch is 'on', it can be represented with a 1.

The order is: Switch 1 value, Switch 2 value, Switch 3 value.

Follow this table:

| Switch 1 | Switch 2 | Switch 3 | Binary Value |
|----------|----------|----------|--------------|
| OFF      | OFF      | OFF      | *000*        |
| OFF      | OFF      | ON       | *001*        |
| OFF      | ON       | OFF      | *010*        |
| OFF      | ON       | ON       | *011*        |
| ON       | OFF      | OFF      | *100*        |
| ON       | OFF      | ON       | *101*        |
| ON       | ON       | OFF      | *110*        |
| ON       | ON       | ON       | *111*        |

If all of the switches are off (000), the switch setting does not need to be specified and the command can be used as normal:

```
relay-exp <channel> <state>
```

**Some Examples:**

The switches are set to on-off-on, setting RELAY0 to on:

```
relay-exp -s 101 0 on
```

The switches are set to on-on-on, setting RELAY1 to off:

```
relay-exp -s 111 1 0
```

The switches are set to off-off-on, setting BOTH relays to on

```
relay-exp -s 001 all 1
```

The switches are set to off-off-off, setting both relays to off

```
relay-exp all off
```


[//]: # (Switch Explanation: I2C Address Mapping)

## I2C Address Mapping

If you're curious about how the dip-switch settings affect the I2C device address of the Relay Expansion, then this table is for you:

| I2C Device Address | Switch Binary Setting |
|--------------------|-----------------------|
| 0x27               | 000                   |
| 0x26               | 100                   |
| 0x25               | 010                   |
| 0x24               | 110                   |
| 0x23               | 001                   |
| 0x22               | 101                   |
| 0x21               | 011                   |
| 0x20               | 111                   |




[//]: # (Switch Explanation: Specifying the Address Directly)

## Using the I2C Address Directly

If you don't like dealing with the switch positions and would rather use the I2C device address directly, this option is for you:

```
relay-exp -a <addr> <channel> <state>
```

Where `<addr>` is the I2C device address. It can be entered with or without the leading `0x`, so `0x27` or `27` will both work.


**Some Examples:**

For a device address of 0x23, set RELAY0 to on:

```
relay-exp -a 0x23 0 on
```

For a device address of 0x26, set RELAY1 to off:

```
relay-exp -a 26 1 0
```

For a device address of 0x24, set BOTH relays to on

```
relay-exp -a 0x24 all 1
```




[//]: # (Using the Libraries)

# Using the Libraries

The C library and Python module will allow you to control the Relay Expansion with your own programs.

For more information, see [this guide](../../Documentation/Libraries/Relay-Expansion-Library).



