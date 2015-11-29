# Using USB Storage with the Omega

*This tutorial is on how to use the USB storage device as separate storage device. If you want to use the USB storage device as Rootfs (i.e. if you want to install `opkg` packages on the USB storage device), then read [[Tutorials/Using-USB-Storage-as-Rootfs]]*.

The Omega can read and write to USB storage devices, such as USB keys, and USB external harddrives.

## Supported Filesystems

The following filesystems are currently supported:

* FAT32
* NTFS
* ext2, ext3, ext4

Let us know if you have any requests!

## Using USB Storage

Steps to setup USB storage:

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