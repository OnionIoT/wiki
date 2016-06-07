##**RPCD Tutorial Part 2**

In this tutorial we will show you how to make your own an executable shell script for the ubus.  We also recommend reading the OpenWRT wiki on the topic aswell, which can be found [here](https://wiki.openwrt.org/doc/techref/rpcd).

Additionally, users should also be familiar with JSON formatting as this is the format used for passing information into UBUS services. Refer to this [link](http://www.w3schools.com/json/) for a brief overview of json formatting.

###**UBUS Shell Scripts**

There are two distinguishing features that seperate ubus shell scripts from regular shell scripts, which we will become evident in our example.

1) I/O

All input/output of _ubus_ shell scripts are in the form of JSON format. This format provides compatibility between all ubus services. JSON is also a very common format for transfering data across the web, so make sure you get comfortable with using it. A typical json object/ file looks something like this.

<pre><code>'{"argument1":"value1", "argument2":"value2","argument3":"value3"}'</code></pre>

2) List and Call functions.

A ubus shell script must also support call and list functions to be attached to the ubus. Remember from the first ubus [tutorial](Hyperlink to the first ubus tutorial), that we used the _ubus_ list function to show us how to use the service and we used the _ubus_ call to actually access the service. Both of these functionalities are described in the shell script.

###**Example**

In our example we will create a simple ubus executable shell script that will compute the addition and subtraction of two integer numbers and return them back in the form of a json.

So to start lets navigate to our rpcd directory.

<pre><code>cd /usr/libexec/rpcd</code></pre>

Now create a file called Math using _cat_

<pre><code>cat > Math</code></pre>

And paste the following code

<pre><code>#!/bin/sh
. /usr/lib/onion/lib.sh  
    #includes the functions that we need to parse JSON files

case "$1" in
    #The list function describes how to use the ubus functions.
        list)
                echo '{ "addition": { "argument1": "value" , "argument2":"value"}, "subtraction": { "argument1": "value", "argument2":"value"} }'
        ;;
    #The call function describes the methods that are available
        call)
                case "$2" in
                        # The addition method
                        addition)
                                # read the argument
                                read input
                                # Load the argument into the json for retrieval
                                json_load "$input"
                                #Using the function below, the value of
                                #"argument1" is stored in variable val1
                                json_get_var val1 "argument1"
                                json_get_var val2 "argument2"
                                #The sum of the two are computed
                                ans=$(($val1+$val2))
                                #The \ is used so that we can substitute
                                #properly
                                echo "{ \"The ans is\" :\"$ans\"}"
                        ;;
                        subtraction)
                                # read the argument
                                read input
                                # Load the argument into the json for
                                json_load "$input"
                                #Using the function below, the value of
                                #"argument1" is stored in variable val1
                                json_get_var val1 "argument1"
                                json_get_var val2 "argument2"
                                #The difference of the two are computed
                                ans=$(($val1-$val2))
                                #The \ is used so that we can substitute
                                #properly
                                echo "{ \"The ans is\" :\"$ans\"}"
                        ;;
                esac
        ;;
esac
</code></pre>

Once you have pasted the code, press the CTRL D and save the file.

Once your done that make the file executable by changing the permission on the file enter this into your command line.

<pre><code>chmod +x Math</code></pre>

after that you'll need to reload the rcpd plugin so that it recognizes the new service we have just attached. To do that either enter this into your command line.

<pre><code>/etc/init.d/rpcd restart</code></pre>

This should attach your service to the ubus.
(if not, try running `rpcd` first with the option `stop` and then with the option `start`)


![RPCD2](http://i.imgur.com/mcdiKW3.png)

Now lets check to see if our service is listed.  To check if your service is available enter _ubus_ list into your command line. If that does not work check out the other options of rpcd by running `/etc/init.d/rpcd` or try restarting your Omega, the Math service should now be connected to the plugin.

![RPCD3](http://i.imgur.com/Ys2ZbeV.png)

As you can see, we have _Math_ listed in our ubus service. Let's try using, go ahead and enter the following command and see if it works.

<pre><code>ubus list -v Math</code></pre>
<pre><code>ubus call Math addition '{"argument1":"4","argument2":"6"}'</code></pre>
<pre><code>ubus call Math subtraction '{"argument1":"4","argument2":"6"}'</code></pre>

![RPCD4](http://i.imgur.com/zKpubtZ.png)

That wraps up our mini-series on Ubus.
