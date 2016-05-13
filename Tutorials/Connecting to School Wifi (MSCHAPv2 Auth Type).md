# Connecting to School Wifi (MSCHAPv2 Auth Type)

In this tutorial, we are going through how to connect to school wifi (using University of Waterloo as an example, i.e. wpa/wpa2 enterprise).
Include a relevant image:

![WP2 Enterprise](http://sw.nohold.net/CiscoSB/Images/id1588_6_wet200securitywpaenterprise.png)


In most universities, the school wifi has both user name and password (i.e. MSCHAPv2 Auth Type), if you are a student or in a place with a log-in system to connect to internet, this tutorial is going to help you out with your fantastic Omega.


[[_TOC_]]


[//]: # (Overview)

# Overview 

Tutorial Difficulty:

**Intermediate**

Time Required:

**5 minutes**

Required Materials:
* The Omega
* The Expansion Dock and Ethernet Expansion(optional)

Useful Experience:
* Openwrt networking


# Background Info

As we know, Omega has build-in wifi, which is very convinent in most cases. However, for some students, they might get some trouble when trying to connect to school wifi, because for this kind of wifi, you need to input username as well. In this tutorial, I am trying to help you to connect to school wifi. That is going to lighten your world.



[//]: # (The Actual Process)

# The Actual Process

First of all, if it is a new Omega, you need to connect to wifi somehow in other places or through ethernet if you live in dorm like me. The reason is that wpad-mini, which is installed in your omega as default, is not capable with the MSCHAPv2 auth type, so you need to upgrade it to wpad, which is more powerful. To do this, you need to connect to internet. If you don't have an ethernet expansion, don't worry, just join a free public wifi and do the following steps.

![ehternet](http://i.imgur.com/9jF7sZ8.jpg?1)

[//]: # (The Steps)

## Step 1: Download `wpad` Package

As I mentioned, connect to internet first, and then run the following command:

```
$ opkg update
$ opkg remove wpad-mini
$ opkg install wpad
```
And then, you can try to modify the network file in `/etc/wireless` to make it work!

[//]: # (Step 2)

## Step 2: Determine the Type of School Wifi

I am assuming you are capable of connecting to school wifi through other device, e.g. laptop, and you can easily find what type of security type is by clicking the network.

![type of security](http://i.imgur.com/7qVsnw1.png)

As the picture above, I am using Linux to find the security type, you can do it in Windows as well. So from the picture you can easily tell the wifi for University of Waterloo(uw) is in wp/wp2 mixed enterprise, and the eap type is Protected EAP(peap). Now what you need to do is from [openwrt networking documentation](https://wiki.openwrt.org/doc/uci/wireless#wpa_modes) to find the one suitable for you if you do not have the same setup with me.

[//]: # (Step 3)

## Step 3: Config Your Wireless File

It is actually easier if you are trying to use `wifisetup` to add the network to the list and then try to modify it rahter than copying all lines of code to make it work. Change your directory to `/etc/config`, and then run `vi wireless`, and then go to the very bottom of the file and you will see the one corresponding to your wifi network:

```
config wifi-iface               
        option device 'radio0'  
        option mode 'sta'       
        option network 'wwan'   
        option ssid 'eduroam'   
        option key 'your password'        
        option disabled '0' 
```

Now, what you need to do is to modify it as what you got from the wiki page:

```
config wifi-iface               
        option device 'radio0'  
        option mode 'sta'       
        option network 'wwan'   
        option eap_type 'peap'  
        option ssid 'eduroam'   
        option encryption 'wpa-mixed'
        option auth 'MSCHAPv2'       
        option identity 'your user name'
        option password 'your password'        
        option disabled '0' 
```
The order doesn't really matter, as long as you have all options in your interface. Just replace my setting with your own which has the same option type.

![setting](http://i.imgur.com/Hjo5he8s.jpg)

[//]: # (Step 4)

## Step 4: Restart the Service

Till now, we are pretty much done with this stuff. You can simply reboot your Omega or run `/etc/init.d/network restart`, and wait for everything done, now, enjoy your internet.




[//]: # (Acknowledgements)

# Acknowledgements

* [openwrt networking documentation](https://wiki.openwrt.org/doc/uci/wireless#wpa_modes)

