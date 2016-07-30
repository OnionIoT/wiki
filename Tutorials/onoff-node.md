# Onoff Node Module for Omega

Using the OnOff Module, you can control the Omega's GPIOs using NodeJS. 

We've made this package available through opkg since installing npm, the regular Node package manager, on the Omega requires the use of external USB storage.

[[_TOC_]]

## Overview

The onoff package allows control of the direction and state of a pin. For further reading on the functionality, refer to the [documentation](https://www.npmjs.com/package/tm-onoff).

## Installation

To install the onoff package run the following commands:

```
opkg update
opkg install onoff-node
```

## Importing into your Node Scripts

The module will be installed to the `/usr/bin/` directory, so you will need to point to the correct directory for import. 

Your import code should look like this:

```js
var Gpio = require('usr/bin/onoff-node/onoff.js').Gpio;
```

## Testing

The following is a test script that will blink an LED connected to GPIO6 every two seconds. 

Let's create a file `test.js`:

```js
var Gpio = require('/usr/bin/onoff-node/onoff.js').Gpio;

var led = new Gpio(6,'out');

var num = 1;

setInterval((function(){
	this.num ^= 1;
	led.writeSync(this.num);
}),2000);

```

Run the script with the following command:

```
node test.js
```

And watch that little LED blink!