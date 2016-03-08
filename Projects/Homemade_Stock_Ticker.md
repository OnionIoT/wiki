###**Homemade Stock Ticker with Omega + OLED Exp**

In this tutorial we will show you how to make a stock ticker using your Omega, Expansion Dock and OLED Expansion, in under 15 minutes. We will be using python for webscraping and basic shell scripting to control the OLED display. We hope that this will inspire you to try your own webscraping projects using the Omega. 

![Final Picture](http://i.imgur.com/tHtyzJS.jpg)

**Pre-requisite: If you haven't already follow the get started [guide](https://onion.io/getstarted) to setup your Omega.**

####**1)Connect your OLED display+Omega+Expansion Dock:**

Firstly, connect your OMEGA and your OLED Expansion to your Expansion Dock. Refer to the picture below.

![Assemble](http://i.imgur.com/RExtrmt.jpg)
![Assembled](http://i.imgur.com/ZsmySUf.jpg)

####**2)Setup Wifi:**

You will need to setup wifi so that we can install python and fetch stock data from google. Go ahead and enter the wifisetup command into your omega. 

<pre><code>wifisetup</code></pre>

Follow the instructions and give the Omega access to your wifi network. 

####**3)Update and Install Python:**

We will need to install the full version of python so that we have access to some libraries necessary for webscraping. To do this simply enter,

<pre><code>opkg update && opkg install python</code></pre>

You may or may not need to clear some space on your Omega for the installation. If you are doing this right out of the box, you should be fine. 

####**4)Create our python and shell scripts.**

Navigate to your "/" directory. Create your shell script file by executing the following command.

<pre><code>cat > stock.sh</code></pre>

You will be prompted to enter the contents of the file. Copy the following code into the file. 

<pre><code>
#!/bin/sh

oled-exp -c
echo $1 > /usr/bin/ticker.txt

VAR=$(./stock_script.py)

oled-exp -i

oled-exp write $VAR

</code></pre>

You can save and exit the command by entering CTRL + D. Firstly, we add the the shell to our path so that we can run the file as an executeable. We then clear the display using _oled-exp -c_. We write the ticker of interest to the following file "/usr/bin/ticker.txt". Next we run our python script and store the output to our string variable, VAR. Then we initialize the display and write the value of VAR, which is our ticker and price. 

Now lets do the same with our python script.

<pre><code>cat > stock_script.py</code></pre>

<pre><code>
#!/usr/bin/env python
import urllib
import json
myfile = open('/usr/bin/ticker.txt', 'r')
rg=myfile.read()
site="http://www.google.com/finance/info?q="+rg
jfile=urllib.urlopen(site)
jsfile=jfile.read()

jsfile=jsfile.replace("\n","")
jsfile=jsfile.replace("/","")
jsfile=jsfile.replace("]","")
jsfile=jsfile.replace("[","")
a=json.loads(jsfile)
ticker=a['t']
price=a['l_fix']
info=ticker+":"+price
print info
</code></pre>

To start we added python to our environment so that it is executable from the command line or a shell script. Next we imported urllib and json libraries, these will allows us to webscrape information and parse the data for our desired information respectively. Next we open the file that stores our desired ticker and store it as a string variable. Then we append that to our URL. Then we will generate the URL and open the file from the web, _urllib.urllopen(site)_. We will store it as a string using _jfile.read()_. Next we will clean it up using the _replace_ functions so that it can be parsed by the json library. We use the _json.load(jsfile)_ to create a dictionary. Then we retrieve the values of interest using the keys for ticker and price. We append the two and spit the output to our shell script.


####**5)Change Permissions**

Make both files we just created executable by entering this:

<pre><code>chmod +x stock.sh stock_script.py </code></pre>

####**6)Execute Script**

Now let's see how Apple is doing.

<pre><code>./stock.sh AAPL</code></pre>

![Final Picture](http://i.imgur.com/7SdtT7s.jpg)


