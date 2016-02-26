# Using the omega to remotely lock your door

## Why would you want to do this?
 - Looking for your key on a keychain is annoying.
 - Having an app to unlock your door is more convenient.
 - You often forget to lock your doors so you can lock them when you're not at home.
 - You want to unlock your door from your car during the winter.

![alt text](http://i.imgur.com/jUdyXu0.jpg "Setup")
![alt text](http://i.imgur.com/8REyJEO.jpg "Setup")

## Materials

  - Omega
  - Expansion Dock
  - Servo Expansion
  - Servo Motor
  - Motor Casing
  - Power supply

## Step 1: The Servo Motor

These [motors] turn using PWM signals. Our [servo expansion] has a pwm-exp function that can be used to generate these pulses. Most deadbolts lock and unlock at 90 degrees so modifications are not usually necessary. If your lock has to turn more than 180 degrees, you may want to modify your motor for [continuous rotation].

## Step 2: The Motor Casing

You will need a way to attach your motor to your door and turn the lock. You can make a casing for the motor that can attach to your door and hold the motor in place. A small piece of plastic in the shape of the lock's bar should be attached to the motor. An easy way to obtain these is to 3D print them. [These] are the designs we are using.

## Step 3: Storing lock state using UCI

[OpenWRT] is installed on the omega so you are able to use [UCI] to store configurations on the omega. So, we will use this to store the status of the lock. If your lock needs to turn more than 180 degrees, this would prevent the motor from applying a force on already locked/unlocked doors.

![alt text](http://i.imgur.com/OYBcm5d.jpg "UCI")

## Step 4: Controlling your servo through ubus

In order to run commands outside of the terminal for an app, you can use [ubus].

Navigate to `/usr/libexec/rpcd/`

The ‘expled’ file is really small so it makes a good template to write scripts. Copy and edit it.
If you modified your motor, the sleep integer may have to be adjusted based on how much it is required to turn. Otherwise, you do not need conditional statements. The script we have basically runs the motor on slot 8 for sleep seconds and set the state of the lock. If you are using UCI, you may want to add a function to force the motor to a position in case the lock status is incorrect. This may happen if someone uses a key to unlock a door so the status is still locked.

[Here] is our code.

## Step 5: Adding ubus Permissions

In order to run ubus commands outside of the terminal, you must first include the functions in the ubus permissions file.

Navigate to `/Onion-Console/acl.d/`

In onion-console.json, add your ubus functions under read:ubus: and write:ubus:

![alt text](http://i.imgur.com/mEznFQo.jpg "read")
![alt text](http://i.imgur.com/v1FaXpz.jpg "write")

## Step 6: Create an app

Having an app to request functions is a lot easier and has a better UI than the terminal. There is an app template at `/www/apps/` which you can use to build an app. Copy the onion-template folder and give it an app name. Edit the app.json and icon.png files to register it. Set the icon value to “true” for your app to show up on the console.

![alt text](http://i.imgur.com/WssGFaL.jpg "template")

Out [example code] for the app is located on GitHub. We are using the [Polymer] framework to create an HTML element. You may look in the [files] of other apps to see how your code should be organized.

This is what the UI looks like.

![alt text](http://i.imgur.com/8VARP3n.jpg "tab 1")
![alt text](http://i.imgur.com/Rdkbnv9.jpg "tab 2")

You are now finished. Attach your motor to your door and test it. Don't forget to reboot your omega!

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
   [These]: <>