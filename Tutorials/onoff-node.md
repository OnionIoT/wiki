# Onoff Node Module for Omega

Using the Onoff package, you can control the Omega's gpios using node. 

[[_TOC_]]

## Installation

To install the onoff package run the following commands:

```
opkg update
opkg install onoff-node
```
## Importing into Script

The module will be installed to the `usr/bin/` directory, so you will need to point to the correct directory for import. Your import code should look like this:

```js
var Gpio = require('usr/bin/onoff-node/onoff.js').Gpio;
```

## Testing

The following is a test script that will blink an led every two seconds. 

Then create a file test.js:

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