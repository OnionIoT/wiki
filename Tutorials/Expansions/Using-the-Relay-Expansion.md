# Using the Relay Expansion

The Relay Expansion allows you to control two relay modules. Relays are basically electronically operated switches, enabling the Omega's low power circuits to control other, potentially high power circuits.


[[_TOC_]]


[//]: # (Hardware)

## The Hardware
The Relay Expansion has two TE Axicom IMO3 relay modules with a switching maximum voltage of 220 VDC and 250VAC, rated for 2A of current, and switching power of 60W, 62.5VA. The switching typically takes 1ms, with a maximum of 3ms.

### The Dip-Switches

The dip switches on the Relay Expansion control the I2C device address of the Expansion, enabling users to stack and control up to eight Relay Expansions with one Omega and Expansion dock.

The 'off' position for each switch is when the toggle is close to the numbers on the switch, or away from the relay modules.

The 'on' position is when the toggle is away from the numbers on the switch, or closer to the relay modules.



[//]: # (Using relay-exp)

## Using the Command Line

Make sure that your Omega has the latest firmware!

A tool we've developed called relay-exp will make controlling your relay expansion easy as pie

### Command Usage

For a print out of the command's usage, run it with just a `-h` argument:

```
relay-exp -h
```

### Initialization

After every power-cycle, the chip on the Relay Expansion must be initialized to correctly and safely control the Relay Modules. **The driver application will automatically detect if initialization is required and perform the required sequence,** so there is no need to run this command.

However, manually triggering the initialization is still available; run the following command:

```
relay-exp -i
```

This can be run on it's own or in conjunction with any commands below.

By default, the relays will be OFF.

### Relay Channels

The Expansion has two modules, this guide will refer to the relays as RELAY0 and RELAY1 or as a channel.

Refer to the following image to identify which channel refers to which relay:

![Relay Expansion](//i.imgur.com/Wk6Z9lW.png)

### Setting a Relay's State

The tool allows you to program the relay's states like so:

```
relay-exp <channel> <state>
```

The channel argument should be either 0 or 1 for RELAY0 and RELAY1 respectively.

The state argument should be 0 or off for turning the relay OFF, or 1 or on for turning the relay ON

When the relay is OFF, it will act as an open switch, so any circuit connected to it will not be closed and will therefore be powered down. When it is ON, it is essentially a closed switch, so connected circuits will be powered on.

A few examples...

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

### Controlling both Relays Simultaneously

You are also able to control both relays in a single command:

```
relay-exp all <state>
```

As above, the state argument should be 0 or off to turn the relay OFF, or 1 or on to turn the relay ON, the only difference is that it will now affect both relays.

Some examples

Initializing the chip and turning both relays ON:

```
relay-exp -i all 1
```

Turning both relays OFF:

```
relay-exp all off
```


[//]: # (Switch Explanation)

## Using Multiple Relay Expansions by Changing the Dip-Switch Settings

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
| OFF      | OFF      | OFF      | 000          |
| OFF      | OFF      | ON       | 001          |
| OFF      | ON       | OFF      | 010          |
| OFF      | ON       | ON       | 011          |
| ON       | OFF      | OFF      | 100          |
| ON       | OFF      | ON       | 101          |
| ON       | ON       | OFF      | 110          |
| ON       | ON       | ON       | 111          |

If all of the switches are off (000), the switch setting does not need to be specified and the command can be used as normal:

```
relay-exp <channel> <state>
```

Some Examples

The switches are set to on-off-on, setting RELAY0 to on:

```
relay-exp -s 101 0 on
```

The switches are set to on-on-on, setting RELAY1 of off:

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


### I2C Address Mapping

If you're curious about how the switches affect the I2C device address of the Relay Expansion, then this table is for you:

| Switch Binary Setting | I2C Device Address |
|-----------------------|--------------------|
| 000                   | 0x27               |
| 001                   | 0x26               |
| 010                   | 0x25               |
| 011                   | 0x24               |
| 100                   | 0x23               |
| 101                   | 0x22               |
| 110                   | 0x21               |
| 111                   | 0x20               |

