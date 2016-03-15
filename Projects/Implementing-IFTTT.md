# Implementing IFTTT

[[_TOC_]]

[//]: # (What is IFTTT)

## What is IFTTT

[IFTTT](https://en.wikipedia.org/wiki/IFTTT) stands for "if this, then that", which is a web service that allows users to create some methods, which are called "recipes", that can set up user's own conditional statement (e.g. if I receive a new email on my gmail account, then forward it to another account). The conditional statement could be as simple as if a button is pressed, or as complex as you want. There is a "do" app in APPstore or Google Play store, which makes users able to trigger a event by pressing a button. Ff you are interested in this project, I strongly recommend you to download it on your phone after you have viewed Theory && Introduction part. 

![IFTTT](http://marketingland.com/wp-content/ml-loads/2012/09/ifttt-logo.jpg)

[//]: # (Theory && Introduction)

## Theory && Introduction

The theory of using IFTTT to control our Omega is, when one event is triggered, it sends a web request to Onion device server, which is interacting with device client (that is what we are going to install on Omega), and then it is going to trigger a ubus function, and the ubus function determines what you are going to do on the Omega. There are three ubus tutorials avaliable on Onion Wiki, click [here](https://wiki.onion.io/Tutorials/OpenWRT%20Tutorials/UBUS_Tutorial/Part1_Ubus_Intro) to create your own ubus function!

The recipes are created through "channels", which are different web services, for example, Gmail. Unfortunately, we haven't created our own channel yet (but it will be up very soon), so we are using IFTTT through a DIY channel, "Maker". Maker channel allows users to make a web request, however, there is no header option. Unfortunately again, to run a program on Omega, we have to send a API format web request to device server, that means, we need to create a local server to translate the request to another with headers. That would be perfect if you can write your own server, if not, don't worry, I created a test server for you! However, you still need a public server to run this local server on.

[//]: # (Start Up)

## Start Up

### Create Onion Account and Device-id

Sorry... The Onion server account and device-id are not currently avaliable for users... Please wait for further updates.

### Installing Device Client

After you have created your Onion account and device id, the next step is to install device client on Omega. You can insntall it using the following command:

```
$wget http://openwrt2.onion.io:8001/device-client_0.1-1_ar71xx.ipk
$opkg install device-client_0.1-1_ar71xx.ipk
```
Now we need to link Omega with the server. Navigate to `/etc/config`, run `vi onion`, add the following configuration.

```
config onion 'identity'
        option deviceId '*'
        option key '*'
```
Replace the asterisk (*) with your device id and api key you have just created, now your configuration should look like the following picture:

![device client configuration](http://i.imgur.com/55jYJgj.png)

Then reboot your device, and run device-client. Then, let's move on.

### Ubus Function

This step, we are going to create our own ubus function on Omega. In the tutorial, we are using the default expansion led as a sample. You can create your own [ubus](https://wiki.onion.io/Tutorials/OpenWRT%20Tutorials/UBUS_Tutorial/Part1_Ubus_Intro) function if you wish!

After you created your own function, we are going to check if the function really works. First, run `ubus call` command, to check if your ubus service is on the list. If not, check whether you make your ubus service excutable or not (`chmod +x your-ubus-service`).

The expled is set as default, so it works anyway, or if you want o check what is going on, you can run the following command to turn off the led:

```
$ubus call expled '{"red":0, "green":0, "blue":0}'
```
Now, you should be able to observe that the led on expansion dock is turned off!

### Running a Local Server

Now, we are going to create the local server. First, we need a public server. If you don't have one, you can apply for one if you want. Then connect to the public server, and install nodejs. Then, we are trying to create a local server, here is the test server for you guys, click [here](https://github.com/xyorever/local_server). Or you can visit [this](http://blog.modulus.io/build-your-first-http-server-in-nodejs) and [this](http://blog.modulus.io/node.js-tutorial-how-to-use-request-module) websites to create your own.

There are two modules that are not installed, so we need to install them to make sure the server can run properly:

```
$npm install httpdispatcher 
$npm install request
```

Then we are trying to test if the server is running properly. Run the server first:

```
$nodejs test_server.js &
[1] 4079
Server listening on: http://localhost:8080
```
Then, we are sending a test request to the server:
```
$curl -X POST localhost:8080/ledoff
/ledoff
Got Post Data
responding: 
200 '{"status":"success"}'
```
Cool! It works all fine.

### Create Recipes
Now, we have something to do with IFTTT.

A IFTTT recipe should look like this:

![IFTTT recipe](http://i.imgur.com/Em0UMPO.png)

Or look like this:

![DO recipe](http://i.imgur.com/W0YJmWs.png?1)

Remember to replace asterisk(*) with your  public server's public ip address.

Then, after the recipe is created, you should be able to trigger you IF statement!

![IFTTT trigger](http://i.imgur.com/tWmNdpU.png)
![DO trigger](http://i.imgur.com/CT5xTSu.png?1)

[//]: # (Colclusion and Better Suggestions)

## Conclusion and Better Suggestions

This is how we implement IFTTT into Omega. Now, we are able to lock or unlock our door by simply pressing a button on your phone! Onion is planning to create a channel for their own, at that time, it is going to be even easier to do it!
