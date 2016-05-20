#Using The GPS Expansion With The Omega

The GPS expansion from Onion features a ublox neo GPS module, which allows seamless integration into your omega projects. The expansion outputs GPS data in the form of NMEA messages, which include all relevant information(latitude, longtitude...etc). For more info on the NMEA standard, refer to this [link](http://www.gpsinformation.org/dale/nmea.htm). We have prepared a package, ogps, to handle the NMEA messages and offer up the relevant information to the user via ubus calls. 

[[_TOC_]]

[//]: # (Hardware Setup)
##Hardware 

Simply connect the GPS expansion into the USB port of your Omega's Dock.

Note: 
You may need to connect the GPS expansion to a USB Hub with its own power supply. Refer to the bottom of the tutorial for more info. 

##GPS Commands

The device driver will already be installed and Linux should recognize the device automatically. To double check, run the following command.

```
ls /dev/
```

![Imgur](http://i.imgur.com/dHn2YfE.png)

You should see a device called "ttyACM0", this is our GPS expansion.

At this point you can read the raw output of the gps, using the following command.

```
cat /dev/ttyACM0
```
This will print out the raw NMEA output. 

![Imgur](http://i.imgur.com/PjMzWWQ.png)


###Installing ogps

You can also use `ogps` to access relevant data offered up by the GPS via ubus calls. To install ogps enter the following commands. 

```
opkg update 
```
```
opkg install opgs
```

![Imgur](http://i.imgur.com/SLxB7QJ.png)

Now that it is installed you can access the information through ubus. To make sure the gps service has initialized, run the command.

```
ubus list 
```
You should see gps listed. 

![Imgur](http://i.imgur.com/iqhoqut.png)


###Usage
Make sure your GPS is connected! To access the data simply run the command.

```
ubus call gps info
```
If the GPS is not locked, the command will return "signal=false". In this case you may need to give the GPS more time to lock onto a satellite or you will need to move to a more open area where the gps can see more satellites. 

If GPS refuses to lock, it may be due to a power stability issue which can be fixed with a hardware adjustment. Refer to the next section of the tutorial to deal with this. 


Otherwise you should have an output that looks like this.

![Imgur](http://i.imgur.com/OHiEx6F.png)

[//]: # (Hardware fix for stability issue)
##Hardware Fix for Stability Issue

If you have given ample time (>10 min) and tested the module in an open area and it still doesn't work, you may need to make a change to your hardware setup. 

To see if this is the case, you need to look at the raw output of the gps expansion. Run the command. 

```
cat /dev/ttyACM0
```

Look for a line that begins with "GPGSV" and "GPRMC". 

Look the third number in the "GPGSV" sentence. This number represents the number of satellites in view, if its abnormally large (~20) and the third position in the "GPRMC" sentence is "V" instead of "A". This hardware adjustment should work for you. 

![Imgur](http://i.imgur.com/9uzG1oO.png)

The issue is that there is a power instability on the GPS expansion. To get around this you will need to connect the GPS expansion into a USB Hub with its own power supply and connect the USB Hub to the Omega's USB Host port

To verify it is working, look at the raw output by using

```
cat /dev/ttyACM0
```
You should see a significant drop in the third number in the "GPGSV" sentence and "A" in the third position of the "GPRMC" sentence. Try running the ubus call gps command again. If it still returns false, try restarting gps service on ubus by running the following command. 

```
/etc/init.d/ugps stop
```

and 

```
/etc/init.d/ugps start
```

Now try running the ubus call for gps info again and it should work. 



