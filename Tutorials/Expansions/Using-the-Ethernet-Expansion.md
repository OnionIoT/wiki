# Using the Ethernet Expansion

The `eth0` interface on the Omega is disabled by default because it is primarily designed as a prototyping platform for wireless applications. However, by changing configuration, you can enable the `eth0` interface on the Omega.

# Step 1. Edit the configuration file

The configuration file for the network is located at `/etc/config/network`

You just need to uncomment or add the following lines:

```
config interface 'lan'
	option ifname 'eth0'
	option type 'bridge'
	option proto 'static'
	option ipaddr '192.168.4.1'
	option netmask '255.255.255.0'
	option ip6assign '60'
```

Save the file.

# Step 2. Restart the network service

Run the following command to restart the network service so that your new configuration will be put into effect:

```
/etc/init.d/network restart
```