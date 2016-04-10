# Using PHP & the Onion Omega

The Onion Omega runs a webserver called uhttpd, this is capable of running PHP, happy days :)

Hidden away in the forums fello onioneers have already worked on setting up basic examples of getting PHP to run & also included basic proof-of-concept scripts. This tutorial walks you through the steps of getting these to work.

![PHP example and the Onion Omega](https://dl.dropboxusercontent.com/u/12816733/onion-omega-php-example-1.png)

**All you need is a little imagination on how you can use & abuse this with you Onion Omega.**

Here are some ideas to get your brain noodling:

1. You could post updates to Thingspeak.com
2. And push updates to PushBullet, so events appear across multiple devices such as a web browser, your iphone, ipad and/or droid
3. Build a fully featured home automation system or security alarm that takes snapshots using a USB web camera

## Right let's get started

If you can manage copy & paste, **you can do this**. 

Let's get started

First we need to install the required packages, to do this you can use the terminal, or ideally via SSH:

```
opkg update
opkg install php5 php5-cgi
```

This installs the required PHP packages.

Optionally you can also install the CLI version of PHP using the command below. This allows you to run PHP scripts from the commandline using "php-cli scriptname.php"

```
opkg install php5-cli
```

Next we need to edit the uhttpd file, you can do this by using this command:

```
vi /etc/config/uhttpd
```

Where you see the 'main' section of text like this:

```
config uhttpd 'main'
        list listen_http '0.0.0.0:80'
        list listen_http '[::]:80'
        list listen_https '0.0.0.0:443'
        list listen_https '[::]:443'
        option redirect_https '1'
        option home '/www'
        option rfc1918_filter '1'
        option max_requests '3'
        option max_connections '100'
        option cert '/etc/uhttpd.crt'
        option key '/etc/uhttpd.key'
        option cgi_prefix '/cgi-bin'
        option script_timeout '60'
        option network_timeout '30'
        option http_keepalive '20'
        option tcp_keepalive '1'
        option ubus_prefix '/ubus'
```

Add this to the last line of that block of text (there is another block of text after this, I've left that out for simplicity)

```
list interpreter ".php=/usr/bin/php-cgi"
option index_page 'index.php'
```

Press 'ESC' on keyboard, and then ':wq' & 'Enter' to save changes.

Now we need to restart the web server, we do this by running this command:

```
/etc/init.d/uhttpd restart
```

And that's it, the uhttpd server is now capable of parsing PHP files.

## Test File

Create a test file & directory by executing the following command:

```
mkdir /www/php
cd /www/php
vi index.php
```

And paste this code into the file & save:

```
<?php 
  echo "hello world, I'm PHP running on my Onion Omega!";
?>
```

Press 'ESC' on keyboard, and then ':wq' & 'Enter' to save changes.

Now open your web browser and go to http://omega-ABCD.local/php/

**Note:** If you receive a directory or file permissions issue, run the following command and try again.

```
chmod -R 755 /www/php
```

## Let's Use the PHP Example from Git Hub

And now to have a play & make things work.

First we need git, so we run the command:

```
opkg update
opkg install git git-http
```

And now swapp directories and download the files:

```
cd /www/php/
git clone https://github.com/ageeweb/php_onion
```

Now in your web browser go to: 

```
http://omega-ABCD.local/php/php_onion/HowToUse.php
```

Obviously shields are needed for the relay, but the RGB LED works fine with the expanded deck and your page will look like this:

![PHP example and the Onion Omega](https://dl.dropboxusercontent.com/u/12816733/onion-omega-php-example-1.png)

## Notes & Further Resources

You can edit the php.ini file by running this command:

```vi /etc/php.ini```

There is another PHP helper you can use here [https://github.com/ringmaster/GPIOHelperPHP](https://github.com/ringmaster/GPIOHelperPHP)

You can read more about this in this community thread: [https://community.onion.io/topic/39/simple-php-web-gpio-example-switching-leds/10](https://community.onion.io/topic/39/simple-php-web-gpio-example-switching-leds/10)


# Using fast-gpio with PHP

Onion Community member [Chris McCaslin](https://community.onion.io/user/chris-mccaslin) developed a PHP wrapper for the `fast-gpio` package.

## Let's Download the Library

First download the library with wget from the gist https://gist.github.com/Immortal-/a18f58ac5c21ba27921b7626b5a8b06e 
``` wget https://gist.githubusercontent.com/Immortal-/a18f58ac5c21ba27921b7626b5a8b06e/raw/df8e70665523c2a06b503954d10943560d5c189f/OmegaPHP.php ```


## Using the Library

The following code shows how the library can be used:

```
<?php
  require 'OmegaPHP.php'; //Require the library from the step above
  
  $gpio = new OmegaPHP();
  //Turns pin 0 to On or HIGH or 1
  $pin = (int)0; //This is just for my testing purposes You do not have to cast to an int.
  
  $gpio->SetPIN($pin,HIGH);// Set's the pin to 1 or HIGH
  $returned = $gpio->ReadPin($pin);
  
  print_r($returned);
  
  // Prints to screen:
  // 1
?>
```

