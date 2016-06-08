# WinSCP for File Transfers on the Onion Omega

Transfering, updating, deleting & creating files via the command-line is fine for small jobs (or really huge jobs), but there are Windows tools out there to make this a lot easier. 

WinSCP is free program that can help you with your daily tasks on the Onion Omega and what you'll learn about in this tutorial.

[[_TOC_]]


# What is WinSCP?

WinSCP is an open source free SFTP client, FTP client, WebDAV client and SCP client for Windows. Its main function is file transfer between a local and a remote computer. Beyond this, WinSCP offers scripting and basic file manager functionality.

In short, it makes creating & updating files on your Onion Omega a lot faster!

# Getting Started

First, download the [WinSCP program from their site](https://winscp.net/eng/download.php).  

Once downloaded, install the application and then run it.

## Configure the Connection

Connecting to the Omega is super simple, you just need to connect to a New Site as shown in the image below:

![WinSCP Image 1](https://dl.dropboxusercontent.com/u/12816733/onion-omega-winscp-1.png)

In the right hand pane, you'll need to set the details for your Onion Omega. Typically these are:

**File Protocol:** SCP

**Hostname:** 192.168.3.1

**Port Number:** 22

**Username:** root

**Password:** onioneer

![WinSCP Image 2](https://dl.dropboxusercontent.com/u/12816733/onion-omega-winscp-2.png)

Once finished, press the "Save" button.

On the next window there is a tickbox option to save the password, thats up to you. Saving the password is less secure, but faster to access your Omega. 

You can name the connection and you can also you can save a desktop shortcut if desired.

![WinSCP Image 3](https://dl.dropboxusercontent.com/u/12816733/onion-omega-winscp-3.png)



## Connecting to Your Onion Omega using WinSCP

Now from the left menu you'll see the name you saved the new site as a few moments ago, eg "root@192.168.3.1", click on this, then "Login".

![WinSCP Image 4](https://dl.dropboxusercontent.com/u/12816733/onion-omega-winscp-4.png)

As this is your first time connecting via WinSCP you'll receive a warning similar to the one below. That's to be expected, click on "Yes"

![WinSCP Image 5](https://dl.dropboxusercontent.com/u/12816733/onion-omega-winscp-5.png)

Congraulations, you now have remote, easy access to your Onion Omega system files!

Happy Hacking!

![WinSCP Image 6](https://dl.dropboxusercontent.com/u/12816733/onion-omega-winscp-6.png)


**Note:** Common sense applies here, just like when you're logged in via a terminal, you are logged in as root aka "god" on your Onion Omega. Go careful :)
