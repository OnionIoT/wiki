# Turning the Omega into a Wi-Fi Range Extender

Even though the Omega has only one physical Wi-Fi interface, you can create two virtual interfaces and have the Omega relay the packets back and forth between the two interfaces. This allows you to set up the Omega as a Wi-Fi range extender that relays the packets between your computer/smartphone and your router. This can be very helpful if your router has a short range and you are having a problem connecting to your router directly from certain rooms in the house. Let's get started!

## 1. Enable the Access Point (AP) on the Omega

Access Point on the Omega should be enabled by default. If it isn't already enabled, you will need to connect to the Omega via the [[serial terminal|Tutorials/Serial-Terminal]]. Then, to enable the Access Point, you will need to edit `/etc/config/wireless`. Uncomment or add the following lines to the file:

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

## 2. Connect the Omega to the router

Next, you will need to connect the Omega to your router. To do this, you will use the `wifisetup` command:

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

## 3. Use Your Omega Wi-Fi Range Extender

At this point, your Omega is connected to router as well as serving its own access point, and the Omega is setup to relay information back and forth between these two Wi-Fi interfaces. This means that you can connect your computer/smartphone to the AP of your Omega, and be able to access the data coming from the router.

To use the Omega as the Wi-Fi range extender, you would typically place the Omega somewhere between your router and your computer/smartphone. Packets will travel from your router to the Omega, and from the Omega to your computer/smartphone instead of directly from the router to the Omega.

Happy hacking!