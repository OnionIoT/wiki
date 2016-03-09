##**RPCD Tutorial Part 1**

Recall in the last Ubus tutorial, we mentioned that some services available on the Ubus are native to OpenWRT and some have been added our onioneers. Well let's take a look again at what we mean. Enter the _ubus_ list command into your command line. 

<pre><code>ubus list</code></pre>

You will see a list of services. But how do we know which ones are custom onion services? Enter the _rpcd_ plugin. Let's take a look back at the figure we introduced in the last tutorial. As the name and figure suggests we are using the rpcd to literally plugin our service into the ubus. This is how are onioneers add service to the ubus and how you can add your custom services to the ubus. 

The rpcd plugin allows users to expose their custom services in the form of executable shell scripts over the ubus. To see what shell scripts are using rpcd, navigate to the _/usr/libexec/rpcd/_ directory and list the contents. 

<pre><code>cd /usr/libexec/rpcd/</code></pre>
<pre><code>ls</code></pre>

![rpcd_list](http://i.imgur.com/FLQl8tL.png)

You can check for yourself, but the same services are available on the ubus. We can use the cat command to open the shell script and see what is going on behind the scenes everytime we call a ubus service. 

In the next tutorial, we will actually show you how to make your own service and attach it to the ubus. 

