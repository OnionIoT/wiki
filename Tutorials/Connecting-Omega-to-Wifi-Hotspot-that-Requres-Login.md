# Connecting the Omega to Wi-Fi Hotspot that Requires Login

![Wi-Fi Login](//i.imgur.com/mXdTw10.jpg)

In order to connect to some Wi-Fi hostspots in certain hotels or restaurants, you will need to first connect to the Wi-Fi and login via your browser before you can access the Internet. Since the Omega does not have a browser, you will need to use your computer or smartphone to login for the Omega. To connect the Omega to one of these Wi-Fi hotspots, following the following instruction:

## 1. Enable the Access Point (AP) on the Omega

Access Point on the Omega should be enabled by default. If it isn't already enabled, you will need to connect to the Omega via the [[serial terminal|Tutorials/Connecting-to-Omega-via-Serial-Terminal]]. Then, to enable the Access Point, you will need to edit `/etc/config/wireless`. Uncomment or add the following lines to the file:

```
config wifi-iface
	option device 'radio0'
	option network 'wlan'
	option mode 'ap'
	option ssid 'Omega AP'
	option encryption 'psk2'
	option key 'onioneer'
```

Then, to restart the Wi-Fi service with the new configuration:

```
wifi
```

Your Access Point should be turned on at this point.

## 2. Connect the Omega to the Wifi router

Next, you will need to connect the Omega to the Wi-Fi hotspot of the hotel/restaurants you are in. To do this, you will use the `wifisetup` command:

```
root@Omega-0104:/# wifisetup
Onion Omega Wifi Setup

Select from the following:
1) Scan for Wifi networks
2) Type network info
q) Exit

Selection:
```

Just follow the instruction to scan the Wi-Fi network and connect to it.

## 3. Use Your Computer/Smartphone to Login for the Omega

At this point, your Omega is connected to the Wi-Fi hotspot for the hotel/restaurant, as well as serving its own access point, and the Omega is setup to relay information back and forth between these two Wi-Fi interfaces. This means that you can connect your computer/smartphone to the AP of your Omega, and be able to access data from the Wi-Fi hotspot.

If you open up a browser and browse to any website, you should be redirected to a page asking you to login, as if you are connected to the Wi-Fi hotspot directly. You simply need to login to the network with your computer, and your Omega should be able to access the Internet as well.