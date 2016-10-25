# Node-Red for the Omega


[[_TOC_]]

## Overview


## Extending Memory and RAM

In order to run node-red or any node module, you will need to extend the Omega's storage and memory with a USB. We recommed ~512Mb of additonal RAM and ~256Mb of additional storage. Follow these tutorials:

[Extending Omega's RAM](https://wiki.onion.io/Tutorials/Extending-RAM-with-a-swap-file)
[Extending Omega's Storage](https://wiki.onion.io/Tutorials/Using-USB-Storage-as-Rootfs)


## Installation

You will need node installed on your Omega. If you don't have it, install it:
```
opkg update
opkg install nodejs
```
And then to install node-red:
```
opkg install onion-node-red
```

## Starting Node-Red

Node red gets installed to the usr/bin directory. To start node-red we will have to execute the red.js file. To do this, we have to navigate to the following directory:

```
cd /usr/bin/onion-node-red/node_modules/node-red/
```

By default, node-red is designed to serve on 'localhost'. To make it accessible, we have to change this to the Omega's Access Point's Ip Address. By default that address is '192.168.3.1'.

To change the IP address change to following line of code inside red.js:
```
settings.uiHost = settings.uiHost||"0.0.0.0";
```
to
```
settings.uiHost = "192.168.3.1"
```
Next, launch node-red with the following command:

```
node red.js
```

To see the UI, you will need to connect to the Omega's Access Point. Once connected, navigate to http://192.168.3.1:1880 and you should see the node-red UI. 