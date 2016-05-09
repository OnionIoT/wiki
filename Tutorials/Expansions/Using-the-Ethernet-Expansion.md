# Using the Ethernet Expansion

The Ethernet Expansion allows your Omega to perform any networking duties via Ethernet. This tutorial will show you how to enable the Ethernet port so that your Omega can act as just another Ethernet device connected to your router or switch. This means that the Omega will depend on the router to assign it an IP address.

The `eth0` interface on the Omega is disabled by default because it is primarily designed as a prototyping platform for wireless applications. However, by changing configuration, you can enable the `eth0` interface on the Omega.

# Step 1. Edit the configuration file

The configuration file for the network is located at `/etc/config/network`

Add the following lines:

```
config interface 'wan' 
   option ifname 'eth0' 
   option proto 'dhcp'   
   option hostname 'OnionOmega'
```

Save the file.

# Step 2. Restart the network service

Run the following command to restart the network service so that your new configuration will be put into effect:

```
/etc/init.d/network restart
```


# Other Applications

The Ethernet Expansion allows for a lot of flexibility in networking with the Omega. Applications range from using the [Omega as a Router](https://wiki.onion.io/Tutorials/Using-Omega-As-A-Router) to using the [Omega as an Ethernet Bridge](https://wiki.onion.io/Tutorials/Using-Omega-As-Wifi-Ethernet-Bridge).

Check out the [Omega and Networking](https://wiki.onion.io/Tutorials/Contents#the-omega-and-connectivity_the-omega-and-networking) section of the Tutorials!
