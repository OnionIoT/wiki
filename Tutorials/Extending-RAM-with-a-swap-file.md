# Extending the Omega's RAM with a Swap File on USB Storage

The Omega comes with 64MB of RAM, while this is a decent amount, you might find yourself in a situation where 64MB is simply not enough! This tutorial will walk you through how you can use USB Storage and a Swap File to extend the amount of RAM available to your Omega.

![Omega + USB Drive](http://i.imgur.com/MpBslLz.jpg)

[[_TOC_]]


[//]: # (What is a Swap File)

# What is Swap File

A Swap File is a special file on a hard drive or flash storage that is used by the operating system to store information not currently being used by the device's RAM. This allows the system and programs to use more memory than just by using the physically available RAM memory. 

Swap Files, therefore, implement virtual memory. This is also known as paging and is used in every modern desktop and mobile operating system to manage and optimize the use of memory. 

For more information, check out the [Paging Wikipedia article](https://en.wikipedia.org/wiki/Paging).



[//]: # (The Procedure)

# The Setup Procedure

Let's get to setting up our very own Swap File and extending the available memory on the Omega!

[//]: # (Install Packages)

## Step 1: Install Required Packages

First, we will install the `swap-utils` package that will allow us to create and use Swap Files:
```
opkg update
opkg install swap-utils
```


[//]: # (Plug in USB)

## Step 2: USB Storage

Plug in a USB drive and make sure it's mounted. For best results, you will need to setup automatic mounting. See the [USB Storage tutorial](./Using-USB-Storage) for details on the procedures.

For the purposes of this tutorial, let's assume the USB device was mounted to `/mnt/sda1`.


[//]: # (Create Swap File)

## Step 3: Create Swap File on the USB storage

Now we need to create the file on the USB drive that will be used as the Swap file. We will be using the `dd` utility to help us create the file. The `dd` utility is meant to convert and copy files, it is very powerful so it can potentially be very destructive if used incorrectly, **stay on your toes when using `dd`**.

The specific `dd` command we will be running will look like:
```
dd if=/dev/zero of=/mnt/sda1/swap.page bs=1M count=256
```

This command will create a file on the USB drive that is 256MB with all of its bytes initialized to `0x00`. This is the file that will be used as the Swap File, so we will effectively be extending our RAM by 256MB. 

Let's go through each part of the command:
* The `if` argument specifies the source of the data, in this case `/dev/zero` is a special system file that outputs as many `0x00` bytes as requested.
* The `of` argument tells `dd` where to create the new file. In our scenario, the USB drive was mounted at `/mnt/sda1` and we want to make a `swap.page` file on the top level of the drive.
* The `bs` and `count` arguments will specify the size of the file that will be created. 
  * The `bs` argument actually stands for Block Size, with a Block being the number of bytes that are read, written, or converted at one time. We are setting our Block size to 1 Million Bytes or 1 Megabyte (MB). 
  * The `count` argument defines how many blocks are to be created. So we are creating 256 blocks that are 1MB each, therefore the total file will be 256MB. 


[//]: # (Setup the Swap File)

## Step 4: Setup the Swap File

Now that the Swap File has been created, we need to tell Linux to setup this file as a swap area:
```
mkswap /mnt/sda1/swap.page
```

Running the `free` command will show that nothing has changed yet:
```
root@Omega-1302:~# free
             total         used         free       shared      buffers
Mem:         61152        42528        18624           96        11648
-/+ buffers:              30880        30272
Swap:            0            0            0
```


[//]: # (Activating the Swap File)

## Step 5: Activate the Swap File

We're finally ready to activate the Swap File and actually expand our available RAM:
```
swapon /mnt/sda1/swap.page
```

Now, when `free` is run again, we will see that the Swap row is populated:
```
root@Omega-1302:~# free
             total         used         free       shared      buffers
Mem:         61152        42644        18508           96        11648
-/+ buffers:              30996        30156
Swap:       262140            0       262140
```

The units of the numbers displayed by the `free` command are kilobytes. The total Swap size is 262140 kilobytes which is equivalent to 256MB, so we can confirm that the Swap File is active and being used as memory.

**Note:** When the Omega is rebooted, the USB Swap Page will no longer be used. Step 5 will have to be repeated after every boot unless some [automatic method]() for activating the Swap Page is created...

[//]: # (LAZAR: populate that link right above)



[//]: # (Going Further)

# Going Further

So it's a little problematic that the Swap File needs to be activated manually after every boot, lets figure out how to automate this tedious little activity.


[//]: # (Automatically Activating the Swap File)

## Automatically Activating the Swap File

The `/etc/rc.local` will be run automatically after every boot, so this is perfect for our purposes.

Add the following to your `/etc/rc.local` file:
```
# activate the swap file on an external USB drive
SWAP_FILE="/mnt/sda1/swap.page"
if [ -e "$SWAP_FILE" ]; then
        swapon /mnt/sda1/swap.page
fi
```

**Make sure this code is placed above the `exit 0` line!**

This will first check that the Swap File exists, and will then activate the Swap File. Note that this depends on the USB drive being automatically mounted during boot, see the [USB Storage tutorial](./Using-USB-Storage) for details on how to accomplish automatic mounting.

Try adding this code, reboot your Omega (with the USB drive still plugged in), and running `free` to confirm the Swap File is being used.


[//]: # (Summary)

# Summary

Using a Swap File allows us to use other types of storage (USB, flash) to extend the amount of RAM available in our system. All modern desktop and mobile operating systems implement swap files in one way or another since storage is generally much cheaper than memory on a density basis. This isn't exactly equivalent to adding more RAM since memory is much, much faster than storage, but for situations where memory usage becomes an issue, this method is incredibly useful. 

Happy hacking!


