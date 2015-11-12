# Setting Up Cross-Compile Environment for Omega

## Step 1: Setup OpenWRT Buildroot Environment

Install tools and library sources:

```
$ apt-get install -y subversion build-essential libncurses5-dev zlib1g-dev gawk flex quilt git-core unzip libssl-dev
```

## Step 2: Get OpenWRT Source

```
$ git clone git://git.openwrt.org/openwrt.git
$ cd openwrt
```

## Step 3: Update Feeds

Feeds are repositories of packages that can be added to the OpenWRT system. Feeds are configured in the feeds.conf.default file. Additional feeds can be added by editing this file. For example, to add the Onion packages, append the following line to the file:

```
src-git onion https://github.com/OnionIoT/OpenWRT-Packages.git
```

Save the ```feeds.conf.default``` file, and update the feeds:

```
$ scripts/feeds update -a
```

This will fetch all the repositories configured in the ```feeds.conf.default``` file.

To install a package to the source we are about to compile, for example:

```
$ scripts/feeds install python
```

This downloads python to our source code tree, but doesn't compile the package by default.

## Step 4: Modify Image

Files in the ```package/base-file/files``` directory will be overwritten to the compiled image at the end of the compilation process. To add custom files to the compiled image, create a files folder in the buildroot directory. Files in this directory will supersede the files in the ```package/base-file/files``` directory.

## Step 5: Select Packages

As mentioned, packages installed are not selected by default when compiling the image, and will not appear in the final firmware. To select packages to be compiled into the image:

```
$ make menuconfig
```

Navigate with ```UP``` and ```DOWN``` keys. ```ENTER``` to enter into a category. To exit to previous menu press ```ESC``` twice. To toggle the selection state of packages press ```SPACE```.

First ensure the following:

- Target System is **Atheros AR7xxx/AR9xxx**
- Subtarget is **Generic**
- Target Profile is **Onion Omega**
- Target Images is **squashfs**

Different markings on packages represent their selection state. ```< >``` are unselected; ```<*>``` are selected; ```<M>``` are selected to compile as modules (i.e. compiled as Opkg packages, but not included in the image by default); ```-*-``` are dependencies required by other package(s).

Selection state can be toggled using ```SPACE```, or ```N``` for unselected, ```Y``` for selected, and ```M``` for module.

When package selection is finished, exit out of menuconfig by pressing ```ESC``` twice. When prompted whether to save changes, select ```Yes```.

## Step 6a: Compile Image

Start the compile process:

```
$ make
```

On multi-core systems, add the ```-j``` flag to take advantage of additional cores:

```
$ make -jn
```

where n is the number of cores (```nproc```) + 1.

To enable verbose mode:

```
$ make V=s
```

Once the compile is complete, binaries are found in the ```bin/ar71xx``` directory. ```openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin``` is the full image, and ```openwrt-ar71xx-generic-onion-omega-squashfs-sysupgrade.bin``` is the image for system upgrade.

Compiled Opkg packages are found in the ```bin/ar71xx/packages``` directory.

## Step 6b: Compile Individual Packages

Compiling the entire image can take a long time depending on the system. Luckily, individual packages can be compiled without needing to compile the whole image.

First, we need to build the toolchain to compile and package the binaries (this is typically done by make automatically when building the image, and it only needs to be done once):

```
$ make tools/install
$ make toolchain/install
```

Now you can compile the individual package:

```
$ make package/<package name>/compile
```

where ```<package name>``` needs to be substituted for the package in question. Also note that if this package has dependencies, they must be compiled first using the same command.

Finally, if you decide the newly compiled package should be installed into the image:

```
$ make package/<package name>/install
```

will install it into the image that you have already created.

Finally, if you wish to generate the index for the package:

```
$ make package/index
```

This is typically done automatically by make to keep an updated list of packages installed in the image.

## Step 7: Flash Image

On the Omega, ```cd``` to ```/tmp```:

```
# cd /tmp
```

This is done because ```/tmp``` is a RAM disk that has enough storage to download the image to be flashed.

Download the image:

```
# wget https://downloads.onion.io/openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin
```

Once the image is downloaded, start the flash process:

```
# sysupgrade openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin
```

If you wish the system to try to preserve modified files in ```/etc``` directory, where most of the configuration is kept, add the ```-c``` flag:

```
# sysupgrade -c openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin
```

If you wish to overwrite all configuration changes and restore the original image setting, add the ```-n``` flag:

```
# sysupgrade -n openwrt-ar71xx-generic-onion-omega-squashfs-factory.bin
```