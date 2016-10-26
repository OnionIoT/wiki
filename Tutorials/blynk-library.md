# Blynk Library for Omega

The Omega now officially supports Blynk and the Blynk Library. You can now use the Blynk app to control your Omega!

[[_TOC_]]


[//]: # (Overview)

## Overview

Using the Blynk Library and Node.js, you can control your Omega through the Blynk mobile app. We have added node.js v4.3.1 as a dependency and will also be installed if you do not have it.

For further reading on the library's functionality refer to the [documentation](https://www.npmjs.com/package/blynk-library) or check out [blynk.cc](blynk.cc).


[//]: # (Installation)

## Installation

To install the Blynk library run the following commands:

```
opkg update
opkg install blynk-library
```

This will install Node.js 4.3.1 as well, if it is not already installed. 

### OnOff Package

If you would like to use Blynk to control the Omega's GPIOs, you will need to install the `onoff` Node package as well:
```
opkg update
opkg install onoff-node
```

### Installation Size

See the table below for info on how much space each of the mentioned packages will take up on your Omega:

|Package|Size|
|-------|----|
|nodejs|8.5MB|
|blynk-library|260kB|
|onoff|880kB|


## Getting Started

Follow along with [Blynk's guide](http://www.blynk.cc/getting-started/) for info how to get started!

[//]: # (Auth Token Setup)

## Optional: Auth Token Check

You can run a quick check to see if your Auth Token works correctly:
``` js
node /usr/bin/blynk-library/bin/blynk-client.js <Auth Token>
```


[//]: # (Importing into Script)

## Using the Library

The modules will be installed to the `/usr/bin/` directory, so you will need to point to the correct directory for import. Your import code should look like this:

```js
var BlynkLib = require('/usr/bin/blynk-library');
```


[//]: # (Installation)

## Testing

The following is a test script to illustrate communication via the Blynk App. On the App side create a button that is a virtual pin 1 (V1)

Then create a file test.js:

```js
var BlynkLib = require('/usr/bin/blynk-library');

var blynk = new BlynkLib.Blynk('<YOUR AUTH TOKEN HERE!>'); // Make sure to replace this with your Auth Token
var v1 = new blynk.VirtualPin(1);

v1.on('write', function(param) {
  console.log('V1:', param);
});

```

And you can run the script with the following command:
```
node test.js
```

Press the button on you app and note the output on the screen. 


You can take this further by using the [OnOff module](https://wiki.onion.io/Tutorials/onoff-node) to control the Omega's GPIOs. 
