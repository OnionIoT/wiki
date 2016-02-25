# Streaming Music with Airplay

[[_TOC_]]

[//]: # (What is Airplay)

## What is Airplay
[Airplay](https://en.wikipedia.org/wiki/AirPlay) is a wireless streaming protocol used by Apple Inc that allows users to stream mainly audio and video between different devices. 

This tutorial shows how to enable Omega with Airplay function so that users could stream audio from other devices (e.g. phones, tablets) and output from Omega.

[//]: # (Basic Setup)

## Basic Setup

To setup Omega with Airplay, we need several devices: a USB cable (not showing up on the picture), an Omega, an Omega dock (mini dock or expansion dock), a music adapter, and a standard music output device (i.e. speakers, earphones).

![components](http://i.imgur.com/6XVbL82.jpg?1)

Connect all the components as following:

![connection](http://i.imgur.com/FBVPnCS.jpg?1)

Now, after installing Shairport-Sync, we are able to listen music from the earphone!


[//]: # (Installing Shairport-Sync onto Omega)

## Install Shairport-Sync onto Omega

To enable Airplay, we are basically installing an opensorce software Shairport-Sync onto Omega. This software makes devices able to play music through Airplay.

### Step 1: Uninstall `avahi-nodbus-daemon`

To install Shairport-Sync, we need to install `avahi-dbus-daemon` first, but there is already `avahi-nodbus-daemon` on Omega, so we need to uninstall it.

```
$ opkg remove avahi-nodbus-daemon --force-depends
```

### Step 2: Download `avahi-dbus-daemon` from openWRT and Install

We can get `avahi-dbus-daemon` from [openWRT download page](https://downloads.openwrt.org/), openWRT is an open source platform, and there are a lot of packages avaliable.Or we can use opkg method, but whichever method we are using, we need to update the package first.

```
$opkg update
$ opkg install avahi-dbus-daemon
```

### Step 3: Download Shairport-Sync and Install

If you would like to install the newest version of [Shairport-Sync](https://github.com/mikebrady/shairport-sync) (it keeps updating) and you know how to build binary package, you can download it from the above website. Else, we can download an already made binary package from a contributor on github (version 2.6.0) or from openWRT download page (version 2.1.15).

openWRT package:

``` 
$ opkg install shairport-sync
```

probonopd (github) package:

```
$ wget https://github.com/probonopd/shairport-sync-for-openwrt/releases/download/20160119/shairport-sync-openssl_2.6-1_ar71xx.ipk
$ opkg install shairport-sync-openssl_2.6-1_ar71xx.ipk
```

### Step 4: Reboot Omega and Run it!

```
$ reboot
```
```
$ shairport-sync 
start up

```

For version 2.1.15, it seems when we are trying to run it, it outputs an error message:

```
$ shairport-sync
start up
could not bind any listen sockets!
```

However, it does not really matter. Even though you are not running it, as long as the Omega is started up, the Airplay option is showing on the phone and you can stream music now!

(If you want learn more features or advanced functions about Shairport-Sync, please visit Airplay series.)

[//]: # (Basic Configuration)

## Basic Configuration

The configuration file for version 2.1.15 is quite different from vesion 2.6. For this tutorial, I am just going to show the very basic of the version 2.1.15 configuration file. If you would like to learn more about version 2.6 or later configuration file, please visit Airplay series.

The configuration file for version 2.1.15 is under the following path: /etc/config/shairport-sync. For 2.6 is under : /etc/shairport-sync.conf 

![configuration](http://i.imgur.com/YqXFSyA.png)

Now you can custom your shairport-sync as you want. For example, if you want to change your Airplay name showing on the devices, you can uncomment it first and change it.

![Onion Audio Player](http://i.imgur.com/GpWEXmn.png)

![Onion Audio Player on phone](http://i.imgur.com/cUUID9a.png?1)

[//]: # (How To Use Airplay)

## How to Use Airplay

Just in case if you do not know how to use Airplay on iPhone or Android, we are providing a simple tutorial for you.

### iPhone users

Due to Airplay is developed by Apple Inc, it is really convenient for iPhone users to use the airplay function.

Step 1. Make sure there is any of the Airplay devices under the same wifi connection, or make sure you are running shairport-sync on Omega.

![Shairport-Sync](//i.imgur.com/xvzfcCy.png)

Step 2. Open the control center on your iPhone. Tap on Airplay button and choose the device that you want to play music on. (If there is no Airplay button, that means there is no Airplay device available.)

![Control Center](//i.imgur.com/GrILOWK.png)

![Airplay](//i.imgur.com/H5c8vAA.png)

![Omega Terminal](//imgur.com/mC6KgRo.jpg)

### Android Users

For Android users, it is not complicated to use Airplay at all. All you need to do is to download an app from Google Play. There are various of Airplay apps avaliable, and what is showing below is one of them named "AllConnect".

Step 1. Download "AllConnect" from Google Play. It is a free software, so you do not need to worry about paying any extra. Make sure there is any of the Airplay devices under the same wifi connection, or make sure you are running shairport-sync on Omega.

![AllConnect](//i.imgur.com/h7QeVhb.png)

![Shairport-Sync](//i.imgur.com/xvzfcCy.png)

Step 2. Open "AllConnect". Tap on the little TV token on the top, and choose the device that you want to play music on.

![All-Airplay](//i.imgur.com/Joy8YwV.png)

![Omega Terminal for Android](//i.imgur.com/LmQcA0j.png)

(If the music drops off, you can try turn off "wifi optimization" option under Settings -> Wlan -> Advanced wi-fi)