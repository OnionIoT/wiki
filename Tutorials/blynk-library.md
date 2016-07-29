# Blynk Library for Omega

We have officially extended our support to the blynk library. You can now use the Blynk app to control your Omega. 

[[_TOC_]]


[//]: # (Overview)

## Overview

Due to the large size of the blynk library and the limited space on the Omega, we have created two seperate packages: blynk-library and blynk-library-lite. The latter does not come with the dependencies namely node-usb,crc,serialport and noble and will take X Mbs once on the omega. We have created this for people who would like to use blynk without using external usb storage. The regular blynk library comes with all the dependencies and should support of Blynk functionality and will occupy X Mb once installed

[//]: # (Installation)

## Installation

To install the regular blynk library run the following commands:

```
opkg update
opkg install blynk-library-node
```

To install the lite version:

```
opkg update
opkg install blynk-library-node-lite
```

## Importing into Script

The modules will be installed to the `usr/bin/` directory, so you will need to point to the correct directory for import. Your import code should look like this:

```js
var BlynkLib = require('usr/bin/blynk-library-node/blynk.js');
```

And if your are using the lite version

```js
var BlynkLib = require('usr/bin/blynk-library-node-lite/blynk.js');
```

## Testing

The following is a test script to illustrate communication via the Blynk App. On the App side create a button that is a virtual pin 1 (V1)

Then create a file test.js:

```js
var BlynkLib = require('usr/bin/blynk-library-node/blynk.js');
 
var blynk = new BlynkLib.Blynk('715f8caae9bf4a91bae319d0376caa8d'); // Replace this with your Auth Key
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
