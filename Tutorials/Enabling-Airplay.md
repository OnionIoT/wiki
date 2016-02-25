# Enabling Airplay

[[_TOC_]]

[//]: # (What is Airplay)

## What is Airplay
[Airplay](https://en.wikipedia.org/wiki/AirPlay) is a wireless streaming protocol used by Apple Inc that allows users to stream mainly audio and video between different devices. 

This tutorial shows how to enable Omega with Airplay function so that users could stream audio from other devices (e.g. phones, tablets) and output from Omega.

[//]: # (Installing Shairport-Sync onto Omega)

## Install Shairport-Sync onto Omega

To enable Airplay, we are basically installing an opensorce software Shairport-Sync onto Omega. This software makes devices able to play music through Airplay.

### Step 1: Uninstall `avahi-nodbus-daemon`

To install Shairport-Sync, we need to install `avahi-dbus-daemon` first, but there is already `avahi-nodbus-daemon` on Omega, so we need to uninstall it.

```
$ opkg update
$ opkg remove avahi-nodbus-daemon --force-depends
```

### Step 2: Download `avahi-dbus-daemon` from openWRT and Install

We can get `avahi-dbus-daemon` from [openWRT download page](https://downloads.openwrt.org/), openWRT is an open source platform, and there are a lot of packages avaliable.

```
$ wget https://downloads.openwrt.org/chaos_calmer/15.05/ar71xx/generic/packages/packages/avahi-dbus-daemon_0.6.31-12_ar71xx.ipk
$ opkg install avahi-dbus-daemon_0.6.31-12_ar71xx.ipk --force-overwrite
```

### Step 3: Download Shairport-Sync and install

If you would like to install the newest version of [Shairport-Sync](https://github.com/mikebrady/shairport-sync) (it keeps updating) and you know how to build binary package, you can download it from the above website. Else, we can download an already made binary package from a contributor on github (version 2.6.0) or from openWRT download page (version 2.1.15).

openWRT package:

``` 
$ wget https://downloads.openwrt.org/chaos_calmer/15.05/ar71xx/generic/packages/packages/shairport-sync_2.1.15-1_ar71xx.ipk
$ opkg install shairport-sync_2.1.15-1_ar71xx.ipk
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

(If you want learn more features or advanced functions about Shairport-Sync, please visit Airplay series)

[//]: # (How To Use Airplay)

## How to Use Airplay

Just in case if you do not know how to use Airplay on iPhone or Android, we are providing a simple tutorial for you.

### iPhone users

Due to Airplay is developed by Apple Inc, it is really convenient for iPhone users to use the airplay function.

Step 1. Make sure there is any of the Airplay devices under the same wifi connection, or make sure you are running shairport-sync on Omega.

[Shairport-Sync](//i.imgur.com/xvzfcCy.png)

Step 2. Open the control center on your iPhone. Tap on Airplay button and choose the device that you want to play music on. (If there is no Airplay button, that means there is no Airplay device available.)

[Control Center](//i.imgur.com/GrILOWK.png)

[Airplay](//i.imgur.com/H5c8vAA.png)

[Omega Terminal](//imgur.com/mC6KgRo.jpg)

### Android Users

For Android users, it is not complicated to use Airplay at all. All you need to do is to download an app from Google Play. There are various of Airplay apps avaliable, and what is showing below is one of them named "AllConnect".

Step 1. Download "AllConnect" from Google Play. It is a free software, so you do not need to worry about paying any extra. Make sure there is any of the Airplay devices under the same wifi connection, or make sure you are running shairport-sync on Omega.

[AllConnect](//i.imgur.com/h7QeVhb.png)

[Shairport-Sync](//i.imgur.com/xvzfcCy.png)

Step 2. Open "AllConnect". Tap on the little TV token on the top, and choose the device that you want to play music on.

[All-Airplay](//i.imgur.com/Joy8YwV.png)

[Omega Terminal2](//i.imgur.com/LmQcA0j.png)