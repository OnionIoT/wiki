# How to Blink Morse Code On the Omega LED

Here's a little fun one!  You can use the LED on the Omega to send messages in Morse code.

To start, you'll need to install the `mod-ledtrig-morse` package:

```
opkg update && opkg install mod-ledtrig-morse
```

Once this is done, a kernel module that can translate text to Morse code and blink the LEDs is automatically installed.  But you still need to tell the kernel which LED you want to blink.  The kernel exposes a lot of hardware status and configuration options through a virtual filesystem under `/sys`.  (I.e. the files under `/sys` aren't *actually* files, but they look and act like files to make it very easy to access them from the command line and in scripts or programs.)

To tell the kernel that we are going to use the new Morse code module, set the LED trigger condition for the Onion system LED to `morse` by using the `echo` command to write the setting into the virtual file:

```
echo morse > /sys/class/leds/onion\:amber\:system/trigger
```

You can verify that it worked by using `cat` to look at the virtual file:

```
root@Omega-12D9:~# cat /sys/class/leds/onion\:amber\:system/trigger 
none timer default-on netdev transient gpio heartbeat [morse] oneshot usbdev phy0rx phy0tx phy0assoc phy0radio phy0tpt
```

You can see that `morse` is selected because it's in brackets.  The other text in that file shows the other available options that this particular bit of the kernel can be set to.

Anyway, now we have everything set up!  We just need to tell the kernel what message to blink on the LED.  Conveniently, once the morse option is selected, the kernel creates a new virtual file for that called (unsurprisingly enough) `message`.  We can use `echo` again to put text there:

```
echo Hello, Onion > /sys/class/leds/onion\:amber\:system/message
```

Now watch your LED!  If it's too fast or too slow, you can change the speed with the `delay` file that also gets created.  E.g.

```
root@Omega-12D9:~# cat /sys/devices/platform/leds-gpio/leds/onion\:amber\:system/delay 50
```
        
That's pretty fast!  Let's slow it down a bit so that people like me who aren't experts can read it:

```
root@Omega-12D9:~# echo 100 > /sys/devices/platform/leds-gpio/leds/onion\:amber\:system/delay
```

The message will keep looping forever or until you change it.  To stop it, you can either clear the message entirely:

```
echo > /sys/class/leds/onion\:amber\:system/message
```

or change the LED trigger to something else:

```
echo default-on > /sys/class/leds/onion\:amber\:system/trigger
```

Feel free to try some of the other options as well!  Remember that you can view the available options by using `cat` as mentioned above.  (The `heartbeat` option is fun too.)

There are many more settings status files available for other devices in `/sys` and it can be a great way to learn about working with hardware under Linux.

## Credit

Thanks to [Fader](blog.ronaldmccollam.com) for writing this awesome tutorial!