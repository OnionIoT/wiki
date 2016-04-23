# Using USB Storage with the Omega

The Omega can read and write to USB storage devices, such as USB keys, and USB external hard-drives. This tutorial will show you how to manually mount and unmount storage, and then how to setup automatic mounting.

![Omega + USB Drive](http://i.imgur.com/MpBslLz.jpg)

*This tutorial is on how to use USB drive as separate storage device. If you want to use the USB storage device as Rootfs (i.e. if you want to install `opkg` packages on the USB storage device), then read [[Tutorials/Using-USB-Storage-as-Rootfs]]*.


[//]: # (Supported Filesystems)

# Supported Filesystems

The following filesystems are currently supported:

* FAT32
* NTFS
* ext2, ext3, ext4

Let us know if you have any requests!



[//]: # (Using Storage)

# Using USB Storage

Steps to setup USB storage manually:

1. Plug in the USB Storage
2. Usually, it will get mapped to the `sda1` device
	* Double check the mapping:
		* `ls /dev/sda*`
	* It should output something like:
		* `/dev/sda  /dev/sda1`
	* Usually it will get mapped to `sda1`
3. Create a mount point directory
	* `mkdir /mnt/sda1`
4. Mount the drive
	* `mount <device> <mount point>`
	* for example: `mount /dev/sda1 /mnt/sda1/`
 	* The storage can now be accessed at `/mnt/sda1`


[//]: # (Safely Removing USB Storage)

## Safely Removing USB Storage

The USB storage must be unmounted before safely removing the disk.

The `umount` command is used to unmount the storage

```
umount <mount point>
```

From the above example:

```
umount /mnt/sda1
```

The USB device can now be safely unplugged.




[//]: # (Automount)

# Automatic Mounting

Manually mounting everytime becomes pretty tedious after a while... Luckily, Linux has a built-in utility to automatically mount drives when they are plugged in.

## Setting up Automatic Mounting for a Drive

Let's take a look at our `fstab` configuration, this is the configuration file that holds all of the storage device info on the system. It can be found at `/etc/config/fstab`, meaning we can access it with [UCI]:
```
root@Omega-267F:~# uci show fstab
fstab.@global[0]=global
fstab.@global[0].anon_swap='0'
fstab.@global[0].anon_mount='0'
fstab.@global[0].auto_swap='1'
fstab.@global[0].auto_mount='1'
fstab.@global[0].delay_root='5'
fstab.@global[0].check_fs='0'
```

**Setup a New Device**

Now, plug in the drive.

Then, we will need to detect the information for the drive and save it in our `fstab` configuration:
```
block detect > /etc/config/fstab
```

Now the Omega has an fstab [UCI] entry for this specific USB drive. Let's update the UCI entry so that it will automatically be mounted.

First, let's see the current configuration by running `uci show fstab`, it will output something like the following:
```
fstab.@global[0]=global
fstab.@global[0].anon_swap='0'
fstab.@global[0].anon_mount='0'
fstab.@global[0].auto_swap='1'
fstab.@global[0].auto_mount='1'
fstab.@global[0].delay_root='5'
fstab.@global[0].check_fs='0'
fstab.@mount[0]=mount
fstab.@mount[0].target='/mnt/sda1'
fstab.@mount[0].uuid='1806-3FEB'	// this is the unique identifier of the USB drive
fstab.@mount[0].enabled='0'
```

Now, lets enable the `mount[0]` device:
```
uci set fstab.@mount[0].enabled='1'
uci commit fstab
```

**Make sure fstab is Enabled**

Just to be safe, let's enable `fstab` to run at boot:
```
/etc/init.d/fstab enable
block mount
```

**Restarting fstab**

Any time the `fstab` configuration is changed, the following command can be used to restart the process so the changes will take effect:
```
block umount;block mount
```

**Summary**

That's it! Now this particular USB drive will be automatically mounted whenever it's plugged in, or if it's present at boot.

Enjoy!


[//]: # (Safely Removing USB Storage)

## Safely Removing USB Storage

Even when automatically mounting a USB drive, it still has to be unmounted before it's unplugged:
```
block umount
```


[//]: # (What's Next)

# What's Next?

Now that your USB drive is mounted, you can use it to store all sorts of different data. For instance, you can set it up so the [Omega's root filesystem is run from the USB device](Using-USB-Storage-as-Rootfs). You can also setup a [Samba share](./Sharing-with-Samba) so that users on your local network can access the USB drive wirelessly. It's really up to you!

Happy hacking!





   [UCI]: <https://wiki.onion.io/Tutorials/OpenWRT%20Tutorials/UCI_Tutorial/uci_introduction>