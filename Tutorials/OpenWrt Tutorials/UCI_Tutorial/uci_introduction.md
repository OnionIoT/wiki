#**Introduction to UCI**

Short for _Unified Configuration Interface_, _uci_ is OpenWRT centralized configuration interface. 

To modify congifuration files we can either navigate to the specific file usually located in the "/etc/config" folder. Alternatively, you can use the _uci_ command line to utility to achieve the same thing. _Uci_ is particular handy when it comes to making modifcations to config files from within a shell script without changing any other parts of the file. For further reading, consult OpenWRT's documentation on the subject which can be found [here](https://wiki.openwrt.org/doc/uci). 

In keeping with our prior tutorials, let's learn about _uci_ through an example. In this tutorial we will secure the Omega's access point with a password. 

###**Example**

To call the uci, from the command line simply type:

<pre><code>uci</code></pre>

The result will give you all the options you can call for uci. 

![uci](http://i.imgur.com/q0wjOFa.png)

Next lets take a closer look at our wireless configuration file, this is the same one that is in our '/etc/config/' folder, but we want to exploit the convenience of using uci. So enter:

<pre><code>uci export wireless</code></pre>

![uci_wireless_export](http://i.imgur.com/4m2ozTr.png)

Or alternatively you can use the _uci show_ command, which will prove useful shortly. 

<pre><code>uci show wireless</code></pre>

![uci_wireless_show](http://i.imgur.com/P3lSCdY.png)

Now in order for us to secure our wifi access point we must change our encryption and add a key. We wil have to use the following command:

<pre><code>uci set &lt;config&gt;.&lt;section&gt;[.&lt;option&gt;]=&lt;value&gt;</code></pre>

Your probably wondering what the second part of the command means. I won't bore you with the details, if your interested refer to OpenWRT [documentation](https://wiki.openwrt.org/doc/uci), but the second part points to the exact line in the config file we would like to change and sets the new value. So in our case we would like to change our encryption from none to psk2. To determine the exact syntax for the second part refer to output of _uci show wireless_. 

Now enter the following into your command line.

<pre><code>uci set wireless.@wifi-iface[0].encryption=psk2 </code></pre>

followed by this, to commit changes:

```
uci commit wireless
```
To observe the changes, re-enter the _uci_ _show_ _wireless_

You should similar changes as in the image below.

![uci_set_encryption_commit](http://i.imgur.com/TJjId9o.png)

Now let's set our password for our key access point. Similar to our previous step we will enter the following command.

<pre><code>uci set wireless.@wifi-iface[0].key=password </code></pre>

followed by 
```
uci commit wireless
```
and to check our changes:
```
uci show wireless
```

![uci_password_wireless](http://i.imgur.com/MBD94Ao.png)

Now to reset your Omega's wifi settings, enter:

```
/etc/init.d/boot reload

```
This will reload your configuration files. You will also need to restart your wifi service. To do that enter this into your command line.

```
wifi
```
Give it a few seconds and your Omega should broadcast a secured wifi signal. To check, open your network page and check avaiable networks, you should your Omega's wifi and it should require the password, which we set as "password" to login. 

![uci_secured_network](http://i.imgur.com/dljTmGC.png)

And voila, you have just used uci to secure your Omega access point. 



### More Information

For more details on UCI, visit the [OpenWRT UCI Technical Reference article](https://wiki.openwrt.org/doc/uci)

Or (faster server) lede-protect | [LEDE Configuration](https://lede-project.org/docs/user-guide/introduction_to_lede_configuration)
