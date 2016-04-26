# How to perform a real factory reset
--- 
We all love our Onion Omega's and there are tons of things you can do with them. But because of the versatility you may find your self in a situation where you would like to start-over and get it to it's original out of the box state. 

## Step one: 
Download the default firmware to your /tmp/ directory (This has a 16mb capacity) using this command.
```
wget http://repo.onion.io/omega/images/omega-v0.0.1-b156.bin /tmp/openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin
```
![Me downloading factory firmware on my webcam server](http://i.imgur.com/YHvakiu.png "Me downloading factory firmware on my webcam server")

## Step two: 
Install the factory firmware with the option to overwrite /etc/
```
sysupgrade -n /tmp/openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin
```
### Warning after following this tutorial YOU WILL NEED TO UPDATE YOUR FIRMWARE. This tutorial will show you how to reset it to factory firmware (0.0.1 b156)

If you would like to skip this updating to newest firmware step than for step one do not use the command 
```
wget http://repo.onion.io/omega/images/omega-v0.0.1-b156.bin /tmp/openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin
```
Replace `http://repo.onion.io/omega/images/omega-v0.0.1-b156.bin` with the newest firmware provided from the [Offical-Repo](http://repo.onion.io/omega/images/)
