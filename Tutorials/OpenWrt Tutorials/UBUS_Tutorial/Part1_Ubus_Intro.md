##**Introduction to UBUS**

This is the first part of a mini series dedicated to introducing users of the Omega to one of the most powerful tools on OpenWRT, the _ubus_. We strongly recommend users to go through our Linux Basics tutorial, found [here](https://wiki.onion.io/pages/Tutorials/LinuxBasics/), prior to proceeding with this tutorial series. Furthermore, users should be comfortable with shell scripting to get the most out of this tutorial. This series may seem challenging, however, completing it is paramount to realizing the full potential of the Omega and your projects.
Without further ado.

#### **_What Is Ubus?_**

The ubus is an interface that allows users to access and use services from the same place. Some services are built-in to OpenWRT and other services are executable files that we have created ourselves. 
##### **Example**
The best way to understand ubus is to study some examples of how we use. 
To start, let's take a look at how to use the ubus. Enter this into your command line.

<pre><code>ubus</code></pre>


![ubus](http://i.imgur.com/w2cLIVW.png)

The command has given us some options, that we can pass in. 

Let's try using the _list_ option to see what services are available to us.

<pre><code>ubus list</code></pre>

![UbusList](http://i.imgur.com/ECAi7m8.png)

You should have a screen that looks like this. Fear not if you have a slightly different list, we may have updated somethings. 

Let's try using the onion service, to do some function. But first we have to what we have functions we have available and how to access them. In general, we can do this by entering this.

<pre><code>ubus list -v servicename</code></pre>

Since, we are interested in what the onion has to offer enter this into the command line. 

<pre><code>ubus list -v onion</code></pre>

You should see a screen like this.

![UbusOnionList.png](http://i.imgur.com/ZYY7mLh.png)

Now lets try to get the omega to blink the LED on the Omega using the heartbeat option. Similar  to how we did it in this [tutorial](https://wiki.onion.io/Tutorials/Blinking-Morse-Code-on-LED). 

To do this we have to use the _call_ option provided by ubus to access the onion service. We then select the function we want and pass the additional parameters using a json format. To re-iterate:

```
ubus call <service> <function> '{<JSON parameters>}'
```

Specifically, enter this into the command line.

<pre><code>ubus call onion omega-led '{"set_trigger":"heartbeat"}'</code></pre>

![ubusOnionHeartbeat](http://i.imgur.com/JnjCute.png)

And see what happen's to the Omega's LED. It should mimic a heartbeat. Now that we have used ubus let's take a more intricate look a what happened behind the scenes.

### **_The Ubus Structure_**

The figure below is a basic outline of how ubus works. 

![ubus_visual](http://i.imgur.com/NStEmL5.png)


In the previous example we connected to _ubus_ via the ubus command line interface, _ubus\_cli_ on the figure. The ubus allowed us connect to onion service through the rcpd, which we will discuss in depth later on. Once the onion service executed the requested function, the ouput was relayed back to the _ubus\_cli_ via the ubus. 

The ubus gives us a very powerful tool for accessing Omega's services both locally and remotely. For example, when we use the Omega as a server, we can use the _httpd-mod-ubus_ service to connect to the Omega's ubus locally. Also if we are using the Omega's cloud, we can connect to the Omega's ubus through the _onion device client_ service. 

