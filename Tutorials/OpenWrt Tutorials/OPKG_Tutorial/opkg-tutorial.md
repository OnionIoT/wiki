##**OPKG Tutorial**

We highly recommend glancing over this tutorial as it will likely help resolving any problems you may have regarding installing new applications.

####_**What is OPKG?**_

_opkg_ is the packge installer and manager used by OpenWRT. Its purpose is akin to the windows installer wizard, the apt-get installer on Ubuntu. Opkg comes connected to a few repositories including our own so that packages can be easily downloaded and installed to your Omega. 

To see which repositories are connected enter the following into the command line. 
<pre><code>cat /etc/opkg/distfeeds.conf.new</code></pre>


####_**OPKG Basics**_

We will look at a few of the more important commands and options when using opkg. Firstly, lets look at _opkg_ update. **Make sure you run this command before trying to install any packages.**

<pre><code>opkg update</code></pre>

This will update opkg list of available packages. If you try to install a package without running _opkg_ _update_ first, opkg may not be able to find the package you are looking for. 

Next to install a particular package, use the following command.

<pre><code>opkg install &lt;packagename&gt;</code></pre>
This will search through the list of available packages. If you would like, you can point the opkg installer to install from a particular repository or from a local directory.

<pre><code>opkg install &lt;urltopackage&gt;</code></pre>
<pre><code>opkg install &lt;pathtopackage&gt;</code></pre>

Sometimes it is also useful to see which packages we have already installed. 
<pre><code>opkg list-installed</code></pre>

You can also upgrade a package or a group of packages using the following command.

<pre><code>opkg upgrade &lt;packages&gt;</code></pre>

As per the OpenWRT wiki, **_this is not recommended_**. For two main reasons:

1) It is far more inefficient at allocating memory than the default installation process.

2)If you are upgrading a kernel package and there are compatibility issues, your device may break. Therefore, **Do Not Upgrade Kernel Packages**. 

Instead of upgrading, it is recommended to reflash OpenWRT with the newer firmware image. 


Lastly, to remove a package, you can use the following command:

<pre><code>opkg remove &lt;packages&gt;</code></pre>