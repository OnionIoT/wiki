# Finding the Omega's IP Address

At one point or another, you will have to find the Omega's IP address, this quick tutorial will show you how!



# Running the Command

Connect to the Omega's terminal and run the `ifconfig` command.

The output will look something like:

![ifconfig terminal](http://i.imgur.com/UhGET9d.png)

In the above scenario, the Omega is connected to a WiFi network and is hosting it's own AP network. The `wlan0` section containts the information with regards to the connected WiFi network, and `wlan0-1` is the information for the Omega's AP network.

The `inet addr` field shows the IP address, in this case the IP address is **192.168.1.226**




### Happy Hacking!

