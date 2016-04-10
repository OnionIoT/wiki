# Using PHP & the Onion Omega v2

The Onion Omega runs a webserver called uhttpd, this is capable of running PHP, happy days :)

Hidden away in the forums fello onioneers have already worked on setting up basic examples of getting PHP to run & also included basic proof-of-concept scripts. This tutorial walks you through the steps of getting these to work.


**All you need is a little imagination on how you can use & abuse this with you Onion Omega.**

 [You Will need to expand your Onion Omega's System File size using a usb stick and pivot-overlay.](https://wiki.onion.io/Tutorials/Using-USB-Storage-as-Rootfs#extroot-with-pivot-overlay)

Let's get started

First we need to install the required packages, to do this you can use the terminal, or ideally via SSH:

```
opkg update
opkg install php5 php5-cgi
```

This installs the required PHP files, which is a text editor and alot easier to use than vim.

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

Press 'ESQ' on keyboard, and then ':wq' & 'Enter' to save changes.

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
	phpinfo();	
?>
```

Press 'ESQ' on keyboard, and then ':wq' & 'Enter' to save changes.

Now open your web browser and go to http://omega-ABCD.local/php/

**Note:** If you receive a directory or file permissions issue, run the following command and try again.

```
chmod -R 755 /www/php
```

## Let's Use the Library

First download the library with wget from the gist https://gist.github.com/Immortal-/a18f58ac5c21ba27921b7626b5a8b06e 
``` wget https://gist.githubusercontent.com/Immortal-/a18f58ac5c21ba27921b7626b5a8b06e/raw/df8e70665523c2a06b503954d10943560d5c189f/OmegaPHP.php ```

Then you may write your own code See my example if you get stumped
## My Eample
https://gist.github.com/Immortal-/c032136484afa32da0e1de5fde76e2f0
## Notes & Further Resources

You can edit the php.ini file by running this command:

```vi /etc/php.ini```
