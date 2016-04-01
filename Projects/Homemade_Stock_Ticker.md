# Homemade Stock Ticker



This main goal of this project is to demonstrate the Omega's capabilities in a practical webscraping application. We will be making a stock ticker using our Omega to retrieve prices and the OLED Expansion to display them. 



![Final Picture](http://i.imgur.com/tHtyzJS.jpg)



We will be using python to do our webscraping as it provides us with a convenient set of tools to retrieve and parse web data as well as a shell script to print our stock information onto the OLED display.







[[_TOC_]]





[//]: # (Overview)



# Overview 



Tutorial Difficulty:



**Beginner**



Time Required:



**15 minutes**



Required Materials:

* The Omega

* The Expansion Dock

* OLED Expansions



Useful Experience:

* Using the OLED Expansion

* Using Python

* Using Shell Scripts

* Scheduling with Cron






# Background Info



Stock Tickers are used display the most recent selling price for a stock on the market. Historically, they have been the most fundamental tool used by "wall streeters" for tracking performance. There are plethora of free stock ticker services for your computer, phone, tablet ...etc. However, we will provide you with the back bone to make a highly customizable stock ticker on a dedicated mini-computer (Omega) and display(OLED Exp) in under 15 min!



[//]: # (The Actual Process)



# The Actual Process



Pre-requisite: If you haven't already follow the get started [guide](https://onion.io/getstarted) to setup your Omega.



<<<<<<< HEAD
=======


All of the code can be found on this GitHub Repo: https://github.com/OnionIoT/oledQrCodeGenerator





>>>>>>> 3e6e7f57bacd69ffc84327133658f6b77ba94984


[//]: # (The Steps)



## Step 1: Connect your Omega, Expansion Dock, and OLED Expansion



Firstly, connect your OMEGA and your OLED Expansion to your Expansion Dock. Refer to the picture below.



![Assemble](http://i.imgur.com/RExtrmt.jpg)

![Assembled](http://i.imgur.com/ZsmySUf.jpg)





[//]: # (Step 2)



## Step 2: Setup Wifi



You will need to setup wifi so that we can install python and fetch stock data from google. Go ahead and enter the wifisetup command into your omega. 



<pre><code>wifisetup</code></pre>



Follow the instructions and give the Omega access to your wifi network.  



[//]: # (Step 3)



## Step 3: Update and Install Python



We will need to install the full version of python so that we have access to some libraries necessary for webscraping. If you have already installed python skip to the next step. To do this simply enter,



<pre><code>opkg update && opkg install python</code></pre>



You may or may not need to clear some space on your Omega for the installation. If you are doing this right out of the box, you should be fine.



[//]: # (Step 4)



## Step 4: Create our Python and Shell scripts



Navigate to your "/" directory. Create your shell script file by executing the following command.



<pre><code>cat > stock.sh</code></pre>



You will be prompted to enter the contents of the file. Copy the following code into the file. 



<pre><code>

#!/bin/sh



oled-exp -c

echo $1 > /usr/bin/ticker.txt



VAR=$(python ./stock_script.py)



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



[//]: # (Step 5)



## Step 5: Change Permissions





Make both files we just created executable by entering this:



<pre><code>chmod +x stock.sh stock_script.py </code></pre>



[//]: # (Step 6)



## Step 6: Execute Script



Now let's see how Apple is doing.



<pre><code>./stock.sh AAPL</code></pre>



![Final Picture](http://i.imgur.com/7SdtT7s.jpg)







[//]: # (Using the Project)



# Using the Project



Parts of this project can and are encouraged to be used in other projects.



## Going Further


To take the project one step further, let's create a stream by having the Omega run the same script once every minute. To do this we will use the _cron_ tool. _cron_ allows us to schedule jobs(commands/programs/script) for execution at a fixed time. First lets schedule our sript to run every minute. To do this, enter the command:

```
crontab -e
```

You will be taken to the vi-editor of the scheduling file which can be found in /etc/crontabs/root. Add the following line to your file and make sure to end the file with an empty line or #.

```
*/1 * * * * /stock.sh AAPL
```

You can save and quit the editor by entering ZZ. 


Using this project as a template, we can go even further with webscraping projects. Maybe build a new headline streamer or an upto date weather tracker. 



## Related Tutorials



If you enjoyed this project, you should check this out [OLED-QR-Code-Generator](https://wiki.onion.io/Projects/OLED-QR-Code-Generator). 




