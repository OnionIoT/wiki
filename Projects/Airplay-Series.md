# Airplay Seires

[[_TOC_]]

[//]:# (Shairport-Sync Update)

## Shairport-Sync Update

To enable Shairport-Sync with some more advanced features, we need to install Shairport-Sync after version 2.4.0. If you know how to build package, you can visit Shairport-Sync](https://github.com/mikebrady/shairport-sync) for the newest version, else, there is an already made package from a contributor on github (version 2.6.0).

You can check your Shairport-Sync version by running the following command:

```
$ shairport-sync -V
```

probonopd (github) package:

```
$ wget https://github.com/probonopd/shairport-sync-for-openwrt/releases/download/20160119/shairport-sync-openssl_2.6-1_ar71xx.ipk
$ opkg install shairport-sync-openssl_2.6-1_ar71xx.ipk
$ reboot
```

```
$ shairport-sync 
start up

```

If you would like to get some more detailed information, you can enable verbose mode by typing `-v`, `-vv`, or `-vvv` upon how detailed information you need.

!(verbose mode)(http://i.imgur.com/mC6KgRo.png)

[//]:# (Configuration File)
## Configuration File

For Shairport-Sync 2.6, the configuration file is quite different from version 2.1.15. If you would like to custom it, you can find the configuration file by typing the following command:

```
$ vi /etc/shairport-sync.conf
```
![configuration file](https://i.imgur.com/dv2ktIc.png)

Now, you can custom your Airplay by changing the conent:

![Onion Audio Player](https://i.imgur.com/sTouUKO.png)

### Setting Password

(!! Caution: After testing, it seems Android devices are not capable for password and the following features, only IOS devices could perfectly run it)

You can easily find there is a password option in the configuration file, if you would like to protect your device, just uncomment it and set it to whatever you like!

![Password](https://i.imgur.com/E6IooIB.png)

### Enable Shairport-Sync with metadata

I am going to show you some of the advanced features of Shairport-Sync, but the first thing you need to do is to enable the metadata. To enable metadata, you just simply need to change the following:

![Metadata](https://i.imgur.com/o7KqdE9.png)

Now, it is time to display something advanced.

[//]:# (Remote Control)

## Remote Control

Sorry... Please wait for further updating...

[//]:# (Audio Track)

## Audio Track

Sorry... Please wait for further updating...