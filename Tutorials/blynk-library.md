# Blynk Library for Omega

We have officially extended our support to the blynk library. You can now use the Blynk app to control your Omega. 

[[_TOC_]]


[//]: # (Overview)

## Overview

Using the blynk library and node.js, you can control your omega through the blynk app, as such you will need node.js installed. 

For further reading on the library's functionality refer to the [documentation](https://www.npmjs.com/package/blynk-library).

[//]: # (Installation)

## Installation

To install the Blynk library run the following commands:

```
opkg update
opkg install blynk-library-node
```



[//]: # (Auth Token Setup)

## Auth Token Setup

You will need to run the Blynk test script to setup your Auth Token.

If you've installed the full Blynk library, run:
``` js
node /usr/bin/blynk-library-node/bin/blynk-client.js <Auth Token>
```


[//]: # (Importing into Script)

## Importing into Script

The modules will be installed to the `/usr/bin/` directory, so you will need to point to the correct directory for import. Your import code should look like this:

```js
var BlynkLib = require('/usr/bin/blynk-library-node/blynk-client.js');
```


[//]: # (Installation)

## Testing

The following is a test script to illustrate communication via the Blynk App. On the App side create a button that is a virtual pin 1 (V1)

Then create a file test.js:

```js
var BlynkLib = require('/usr/bin/blynk-library-node');

var blynk = new BlynkLib.Blynk('728e8d997ade4cd1b8c50b6e8a63b1d7'); // Replace this with your Auth Key
var v1 = new blynk.VirtualPin(1);

v1.on('write', function(param) {
  console.log('V1:', param);
});

```

And you can run the script with the following line.
```
node test.js
```

Press the button on you app and note the output on the screen. 

You can take this further by using the [OnOff module](https://wiki.onion.io/Tutorials/onoff-node) to control the Omega's GPIOs. 
