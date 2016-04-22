
# Creating a Wireless, Automatic Door Lock

This guide will show you how to use the Omega to build an internet-enabled automatic lock for any door with a single cylinder deadbolt. 

We will be taking a regular deadbolt like this:
![old lock](http://i.imgur.com/t8WYeFG.jpg "old")

And making it wirelessly controllable:
![alt text](http://i.imgur.com/jUdyXu0.jpg "Setup")
![alt text](http://i.imgur.com/8REyJEO.jpg "Setup")

This guide will show you how to setup the physical mechanism to lock & unlock the deadbolt, and the software required to control the mechanism wirelessly.


[[_TOC_]]

[//]: # (Overview)

# Overview

Tutorial Difficulty:

**Intermediate**

Time Required:

**15-20 minutes**

Required Materials:
* The Omega
* The Expansion Dock
* The Servo Expansion
* A Servo Motor
* A Motor Casing
* A MicroUSB Power supply

Useful Experience:
* Using the Servo Expansion
* Using Shell Scripts
* Using JavaScript
* Familiarity with the Omega



# Background Info
A servomotor is a device that will position itself at a specific angular position based on a pulse-width modulated signal. The Onion Servo Expansion was specifically created to control servomotors. Using this combination, we will be able to turn a deadbolt lock at will.



[//]: # (The Actual Process)

# The Actual Process

We are going to prepare the servo, accessories, and hardware. After that, we will configure the commands to activate the servo and then create an app that makes use of the command.

All of the code can be found on this GitHub repo:
https://github.com/OnionIoT/smart-lock/tree/master/tutorial

[//]: # (The Servo Motor)

# Step 1: The Servo Motor

These [motors] turn using PWM signals. Our [servo expansion] has a pwm-exp function that can be used to generate these pulses. Most deadbolts lock and unlock at 90 degrees so modifications are not usually necessary. If your lock has to turn more than 180 degrees, you may want to modify your motor for [continuous rotation].

[//]: # (The Motor Casing)

# Step 2: The Motor Casing

You will need a way to attach your motor to your door and turn the lock. You can make a casing for the motor that can attach to your door and hold the motor in place. A small piece of plastic in the shape of the lock's bar should be attached to the motor. An easy way to obtain these is to 3D print them. [These] are the models we are using.

[//]: # (Storing lock state using UCI)

# Step 3: Storing lock state using UCI

[OpenWRT] is installed on the omega so you are able to use [UCI] to store configurations on the omega. So, we will use this to store the status of the lock. If your lock needs to turn more than 180 degrees, this would prevent the motor from applying a force on already locked/unlocked doors.

![alt text](http://i.imgur.com/OYBcm5d.jpg "UCI")

[//]: # (Controlling your servo through ubus)

# Step 4: Controlling your servo through ubus

In order to run commands outside of the terminal for an app, you can use [ubus].

Navigate to `/usr/libexec/rpcd/`

The ‘expled’ file is really small so it makes a good template to write scripts. Copy and edit it.
If you modified your motor, the sleep integer may have to be adjusted based on how much it is required to turn. Otherwise, you do not need conditional statements. The script we have basically runs the motor on slot 8 for sleep seconds and set the state of the lock. If you are using UCI, you may want to add a function to force the motor to a position in case the lock status is incorrect. This may happen if someone uses a key to unlock a door so the status is still locked.

[Here] is our code.

[//]: # (Adding ubus Permissions)

# Step 5: Adding ubus Permissions

In order to run ubus commands outside of the terminal, you must first include the functions in the ubus permissions file.

Navigate to `/usr/share/rpcd/acl.d/`

In onion-console.json, add your ubus functions under read:ubus: and write:ubus:

![alt text](http://i.imgur.com/mEznFQo.jpg "read")
![alt text](http://i.imgur.com/v1FaXpz.jpg "write")

[//]: # (Creating an App)

# Step 6: Create an app

Having an app to request functions is a lot easier and has a better UI than the terminal. There is an app template at `/www/apps/` which you can use to build an app. Copy the onion-template folder and give it an app name. Edit the app.json and icon.png files to register it. Set the icon value to “true” for your app to show up on the console.

![alt text](http://i.imgur.com/WssGFaL.jpg "template")

Our [example code] for the app is located on GitHub. We are using the [Polymer] framework to create an HTML element. You may look in the [files] of other apps to see how your code should be organized.

This is what the UI looks like.

![alt text](http://i.imgur.com/8VARP3n.jpg "tab 1")
![alt text](http://i.imgur.com/Rdkbnv9.jpg "tab 2")

You are now finished. Attach your motor to your door and test it. Don't forget to reboot your omega!



[//]: # (Using the Project)

# *Using the Project*

Simply press the buttons on your app to tell the servo to lock or unlock your door.

## *Going Further*

You may want to connect your app to the web so it can be used from outside of your home's wifi signal.

   [motors]: <http://www.jameco.com/jameco/workshop/howitworks/how-servo-motors-work.html>
   [servo expansion]: <https://wiki.onion.io/Tutorials/Expansions/Using-the-Servo-Expansion>
   [continuous rotation]: <https://www.flickr.com/photos/randomskk/2569969633/in/photostream/>
   [OpenWRT]: <https://wiki.openwrt.org/>
   [UCI]: <https://wiki.openwrt.org/doc/uci>
   [ubus]: <https://wiki.openwrt.org/doc/techref/ubus>
   [Polymer]: <https://www.polymer-project.org/1.0/docs/start/getting-the-code.html>
   [files]: <https://github.com/OnionIoT/Onion-Console/tree/master/www/apps>
   [Here]: <https://github.com/OnionIoT/smart-lock/blob/master/tutorial/rpcd/onion-lock>
   [example code]: <https://github.com/OnionIoT/smart-lock/blob/master/tutorial/onion-lock>
   [These]: <https://github.com/OnionIoT/smart-lock/tree/master/tutorial/models>
