# Adding your Public Key to the Omega

[[_TOC_]]

## What is a Public Key

A public key is one part of a cryptographic algorithm that is used to secure digital communications. **On the Omega specifically, a public key can be setup so that you don't have to type your password for everytime you use SSH or an SSH-based service (rsync, scp, sftp, etc)**. For more, information on public and private keys, check out the [Wikipedia article on this subject](https://en.wikipedia.org/wiki/Public-key_cryptography), it goes into a lot of detail


## Setup Steps

### Find or Generate your Public Key.

**Find an Existing Key**

If you have a key already generated on a Unix based system, it should be in 
```
~/.ssh/id_pub.rsa
```

**on debian based linux distributions it is in:**
```
~/.ssh/id_rsa.pub
```
change to the ```~/.ssh/``` directory and copy the key direct to the omega,```if the file (authorized_keys) not exists jet```, with:
```
scp id_rsa.pub root@192.168.3.1:/etc/dropbear/authorized_keys
```

**Optionally Generate a Key**

If not, follow these guides for [OS X](https://help.github.com/articles/generating-an-ssh-key/#platform-mac), [Windows](https://help.github.com/articles/generating-an-ssh-key/#platform-windows), and [Linux](https://help.github.com/articles/generating-an-ssh-key/#platform-linux).

**Copy the contents of the key to your clipboard**

### Add your public key to the Omega

Create the following file:
```
/etc/dropbear/authorized_keys
```

Copy and paste your public key into this file.

**Now your computer will be able to SSH with the Omega without a password!**
