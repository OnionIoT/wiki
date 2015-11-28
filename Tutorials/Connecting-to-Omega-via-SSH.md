# Connect to the Omega via SSH

The Omega can be accessed with a [[serial terminal|Tutorials/Connecting-to-Omega-via-Serial-Terminal]] when it is plugged into your computer.

You can also use SSH to connect to the Omega as long as it's on and your computer is on the same Wi-Fi network. SSH is cool because it allows you to connect to your Omega wirelessly!

If your computer is on the same wifi network as the Omega, the Omega will be accessible via the omega-ABCD.local address.
Where ABCD is 4 hex characters that are unique to each Omega and can be seen on the Omega's default AP name or on the Omega command line (seen through a serial connection).

## OSX & Linux (Or other *NIX operating system)

**Step 1**: Open the Terminal app
**Step 2**: Run the following command:
```
$ ssh root@omega-ABCD.local
```

When prompted, enter the password
By default, the password is: onioneer
And you're in

## Windows

**Step 1**: Download and install Putty from [[http://the.earth.li/~sgtatham/putty/latest/x86/putty-0.65-installer.exe]].
**Step 2**: Configure an SSH connection to `omega-ABCD.local` on port `22`:

![SSH into the Omega](//i.imgur.com/dTrdrq9.png SSH into the Omega)

**Step 3**: Click Open, enter the credentials.

By default, the credentials are:

**Username**: root
**Password**: onioneer

And you're connected.