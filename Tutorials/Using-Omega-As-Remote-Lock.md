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

You will need a way to attach your motor to your door and turn the lock. You can make a casing for the motor that can attach to your door and hold the motor in place. A small piece of plastic in the shape of the lock's bar should be attached to the motor.

## Step 3: Storing lock state using UCI

[OpenWRT] is installed on the omega so you are able to use [UCI] to store configurations on the omega. So, we will use this to store the status of the lock. If your lock needs to turn more than 180 degrees, this would prevent the motor from applying a force on already locked/unlocked doors.

![alt text](http://i.imgur.com/OYBcm5d.jpg "UCI")

## Step 4: Controlling your servo through ubus

In order to run commands outside of the terminal for an app, you can use [ubus].

Navigate to `/usr/libexec/rpcd/`

The ‘expled’ file is really small so it makes a good template to write scripts. Copy and edit it.
If you modified your motor, the sleep integer may have to be adjusted based on how much it is required to turn. Otherwise, you do not need conditional statements. The script we have basically runs the motor on slot 8 for sleep seconds and set the state of the lock. If you are using UCI, you may want to add a function to force the motor to a position in case the lock status is incorrect. This may happen if someone uses a key to unlock a door so the status is still locked.

Here is our code.

```bash
#!/bin/sh

device="door"

jsonLock='"lock": { }'
jsonUnlock='"unlock": { }'
jsonForcelock='"forcelock": { }'
jsonForceunlock='"forceunlock": { }'
jsonStatus='"status": { }'

case "$1" in
    list)
		echo "{ $jsonLock, $jsonUnlock, $jsonForcelock, $jsonForceunlock, $jsonStatus }"
    ;;
    call)
		case "$2" in
			lock)
				# rotate left
				if [ `uci get door.@door[0].lock` = '1' ]; then
					echo '{ "status": "already locked" }'
				else
					pwm-exp -i
					pwm-exp -p 8 1.8 20
					sleep 2
					pwm-exp -i
					uci set door.@door[0].lock=1
					uci commit door
					echo '{ "status": "locked" }'
				fi
			;;
			unlock)
				# rotate right
				if [ `uci get door.@door[0].lock` = '1' ]; then
					pwm-exp -i
					pwm-exp -p 8 1.2 20
					sleep 2
					pwm-exp -i
					uci set door.@door[0].lock=0
					uci commit door
					echo '{ "status": "unlocked" }'
				else
					echo '{ "status": "already unlocked" }'
				fi 
			;;
			forcelock)
				# rotate left
				pwm-exp -i
				pwm-exp -p 8 1.8 20
				sleep 2
				pwm-exp -i
				uci set door.@door[0].lock=1
				uci commit door
				echo '{ "status": "locked" }'
			;;
			forceunlock)
				# rotate right
				pwm-exp -i
				pwm-exp -p 8 1.2 20
				sleep 2
				pwm-exp -i
				uci set door.@door[0].lock=0
				uci commit door
				echo '{ "status": "unlocked" }'
			;;
			status)
				if [ `uci get door.@door[0].lock` = '1' ]; then
					echo '{ "status": "locked" }'
				else
					echo '{ "status": "unlocked" }'
				fi
		;;
		esac
    ;;
esac
```

## Step 5: Adding ubus Permissions

In order to run ubus commands outside of the terminal, you must first include the functions in the ubus permissions file.

Navigate to `/Onion-Console/acl.d/`

In onion-console.json, add your ubus functions under read:ubus: and write:ubus:

![alt text](http://i.imgur.com/mEznFQo.jpg "read")
![alt text](http://i.imgur.com/v1FaXpz.jpg "write")

## Step 6: Create an app

Having an app to request functions is a lot easier and has a better UI than the terminal. There is an app template at `/www/apps/` which you can use to build an app. Copy the onion-template folder and give it an app name. Edit the app.json and icon.png files to register it. Set the icon value to “true” for your app to show up on the console.

![alt text](http://i.imgur.com/WssGFaL.jpg "template")

Here is our code for the app. We are using the [Polymer] framework to create an HTML element. You may look in the [files] of other apps to see how your code should be organized.

```javascript
<link rel="import" href="/lib/polymer/polymer.html">
<link rel="import" href="/lib/iron-input/iron-input.html">
<link rel="import" href="/lib/paper-tabs/paper-tabs.html">
<link rel="import" href="/lib/iron-pages/iron-pages.html">

<dom-module id="onion-lockapp">

	<template>
		<style>
			paper-tab {
				background-color: #5cd65c;
				width: 50%;
				color: white;
				font-size: 20pt;
			}

			button {
				width: 50%;
				height: 100%;
				margin-right: -4px;
				border: none;
			}

			div {
				height: 100%;
			}

			img {
				max-height: 40%;
			}

			#lock {
				background-color: #f1c11e;
			}

			#unlock {
				background-color: white;
			}

			#forcelock {
				background-color: #828e9e;
			}

			#forceunlock {
				background-color: white;
			}

		</style>
	
		<div id="container">
			<paper-tabs selected="{{page}}">
				<paper-tab on-click="_changePage(1)">{{_getStatus(isLocked)}}</paper-tab>
				<paper-tab on-click="_changePage(0)"><img src="force.png" style="max-height:100%"></img></paper-tab>
			</paper-tabs>

			<iron-pages selected="{{page}}" style="height:100%">
				<div>
					<button id="lock" on-click="_lock"><img src="lock.png"></img></button>
					<button id="unlock" on-click="_unlock"><img src="key.png"></img></button>
				</div>

				<div>
					<button id="forcelock" on-click="_forceLock"><img src="tape.png"></img></button>
					<button id="forceunlock" on-click="_forceUnlock"><img src="unlock.jpg"></img></button>
				</div>
			</iron-pages>
		</div>
	</template>

	<script>

		(function () {
			"use strict";

			var _changePage = function(x) {
				switch(x) {
					case 0:
						this.page = "1";
						break;
					case 1:
						this.page = "0";
						break;
					default:
						this.page = "1";
				}
			};

			var _lock = function() {
				this.isLocked = true;
				onionConsole.getService('onion-ubus-provider', (function (ubus) {
					ubus.request('onion-lock', 'lock', {}, (function (data) {}).bind(this));
				}).bind(this));
			};

			var _unlock = function() {
				this.isLocked = false;
				onionConsole.getService('onion-ubus-provider', (function (ubus) {
					ubus.request('onion-lock', 'unlock', {}, (function (data) {}).bind(this));
				}).bind(this));
			};	

			var _forceLock = function() {
				this.isLocked = true;
				onionConsole.getService('onion-ubus-provider', (function (ubus) {
					ubus.request('onion-lock', 'forcelock', {}, (function (data) {}).bind(this));
				}).bind(this));
			};

			var _forceUnlock = function() {
				this.isLocked = false;
				onionConsole.getService('onion-ubus-provider', (function (ubus) {
					ubus.request('onion-lock', 'forceunlock', {}, (function (data) {}).bind(this));
				}).bind(this));
			};

			var _getStatus = function(x) {
				if(x) {
					return "Locked";
				}
				return "Unlocked";
			};

			Polymer({
				is:"onion-lockapp",
				_changePage : _changePage,
				_lock : _lock,
				_unlock : _unlock,
				_forceLock : _forceLock,
				_forceUnlock : _forceUnlock,
				_getStatus : _getStatus,
				properties: {
					isLocked: {
						type: Boolean,
						value: false,
						notify: true,
						reflectToAttribute: true
					},
					page: {
						type: String,
						value: "0"
					},
					ready: function() {
						onionConsole.getService('onion-ubus-provider', (function (ubus) {
							ubus.request('onion-lock', 'status', {}, (function (data) {}).bind(this));
						}).bind(this));
					}
				},

			});
		})();
	</script>

</dom-module>
```

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