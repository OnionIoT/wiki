# Updating the Firmware on the Omega

The Onion team will be releasing firmware updates regularly to provide new features and to fix any issues.

## Omega Firmware Identification

The firmware will be identified by a version code and a build number.
The version code is in the format `X.Y.Z`
The build number is an integer number

For example: `0.0.2 b170`

### Important to Note

Upgrading the firmware will delete/overwrite all of the files that are not in `/etc` or `/usr`

We're comping up with way to update without nuking all the files, stay tuned!


## Updating with the Command Line:

The firmware can be updated using the `oupgrade` tool.

To check your device's version against the latest, do the following:

```
oupgrade -check
```

This will produce output like the following:

```
> Device Firmware Version: 0.0.2 b158
> Checking latest version online...
> Repo Firmware Version: 0.0.2 b170
> Comparing version numbers
> New build of current firmware available, upgrade is optional, rerun with '-force' option to upgrade
```

If there is a new firmware version, run the following command to update:

```
oupgrade
```

If the latest firmware is the same version but there is a newer build number, the update is not mandatory.

If you choose to update, run the following command:

```
oupgrade -force
```

The above commands will start the download of the new firmware from our servers, and will then initiate the update.
**DO NOT UNPLUG THE OMEGA DURING THE UPDATE**!

Once the Omega has rebooted, the update has completed and your Omega is ready for fun!


## Updating with the Console:

Open the Settings App, open the Update & Restore pane, click the Upgrade button.

This will start the the download of the new firmware from our servers, and will then initiate the update.
**DO NOT UNPLUG THE OMEGA DURING THE UPDATE**!

Once the Omega has rebooted, the update has completed and your Omega is ready to go!

# Getting back to a Factory Image Omega

We all love our Omegas and there are tons of things you can do with them. But because of the versatility you may find your self in a situation where you would like to start-over and get it to it's original out of the box state. To do this it takes an operational Omega. 

*Credits to [ChrisMcCaslin](https://community.onion.io/user/chris-mccaslin) for contributing this section*

## Step 1:
Download the default firmware to your '/tmp/' directory (this is the Omega's RAM) using this command:
```
wget http://repo.onion.io/omega/images/omega-v0.0.1-b156.bin /tmp/openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin
```
![Me downloading factory firmware on my webcam server](http://i.imgur.com/YHvakiu.png "Me downloading factory firmware on my webcam server")

## Step 2:
Install the factory firmware with the option to overwrite `/etc/`
```
sysupgrade -n /tmp/openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin
```

## Step 3:
Now your Omega is on the factory firmware (0.0.1 b156). You will need to update your firmware to get all of the latest features made available by Onion!

