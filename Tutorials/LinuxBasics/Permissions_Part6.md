##**Ownership and Permissions**




In all linux systems there is a hierarchy of users, with the root, also known as super user, sitting at the top. Each of the users have an ownership over their _own_ files and have the right to read, write and execute them as they please. Users do not have the same permissions to other users files. The exception to this is the super user or root. On the Omega, we will not concern ourselves with the user system since we are always logged in as root. 



To further explain the concept, let's revisit the last [tutorial](https://github.com/OnionIoT/wiki/blob/master/Tutorials/LinuxBasics/ShellScript_Part5.md) on scripting. 



In our last tutorial we executed the shell script by using the :



<pre><code>sh LogGen.sh </code></pre>



Now that command worked by let's say we wanted to drop the sh and run the command:



<pre><code> ./LogGen </code></pre>



We will receive this message from the command line



<pre><code>/bin/ash: ./LogGen.sh: Permission denied</code></pre>



So lets go ahead and look at the permission on the shell script. To do that run the ls command with the -l option. This will give us a detailed description of the files/subdirectories in our current directory.



<pre><code>ls -l</code></pre>



You should see something like this...
```
root@Omega-24CF:/# ls -l
-rw-r--r--    1 root     root           985 Sep  3 16:28 LogGen.sh
drwxr-xr-x    2 root     root           722 Jul  8 19:36 bin
drwxr-xr-x    5 root     root           920 Aug 18 18:07 dev
drwxrwxr-x    1 root     root             0 Jul  8 19:33 etc
drwxr-xr-x   11 root     root           807 Jul  8 19:26 lib
-rw-r--r--    1 root     root           170 Sep  3 16:50 log.txt
drwxr-xr-x    2 root     root             3 Jul  8 19:30 mnt
drwxr-xr-x    5 root     root             0 Jan  1  1970 overlay
dr-xr-xr-x   51 root     root             0 Jan  1  1970 proc
drwxrwxr-x   16 root     root           235 Jul  8 19:36 rom
drwxr-xr-x    1 root     root             0 Sep  3 14:08 root
drwxr-xr-x    2 root     root           742 Jul  8 19:36 sbin
dr-xr-xr-x   11 root     root             0 Jan  1  1970 sys
drwxrwxrwt   16 root     root           480 Sep  3 17:04 tmp
drwxr-xr-x    1 root     root             0 Aug  5 20:13 usr
lrwxrwxrwx    1 root     root             4 Jul  8 19:36 var -> /tmp
drwxr-xr-x    9 root     root           171 Jul  8 19:36 www
root@Omega-24CF:/# 
```


For now all we are concerned with is the column containing the stuff that looks like this drwxr-xr-x . If you are interested in a more detailed description of what all the descriptions mean refer to this [article](https://www.linux.com/learn/tutorials/309527-understanding-linux-file-permissions). 



So lets take a look at our permission for the LogGen.sh file. Yours should look like this.



<pre><code>-rw-r--r--</code></pre>



The first "-" indicates that it is a file, if it were a directory we could make we would find a "d" in its place. The next three characters indicate whether the super user has read,write,executable permission. Similarly the next 6 characters are similar indicators for the user and the usergroup, with "rwx" indicating full permission and "---" indicating no permission. 



####_chmod_ command



Since we are the root user we can give ourselves full permission to the file. So let's go ahead and change the permissions. To do this we will need to employ the  _chmod_ command. At this point, quickly read this [article](http://linuxcommand.org/lts0070.php) describing how to use the command.





To give ourselves (root) full permission 755 would be ok. If we want to include users in the (root group) 775 would be the right permission. As we want to keep our system as secure as possible, we never should use 777 as full permission. This would mean everyone (group other) who has acces to the system, can run/execute a script wich could be fatal.


<pre><code>chmod 755 LogGen.sh</code></pre> just for User root
<pre><code>chmod 775 LogGen.sh</code></pre> for root and group root (in our example).


To check if the permission has changed enter:



<pre><code>ls -l</code></pre>



You should see something like this below.
```bash
root@Omega-24CF:/# ls -l
-rw-r--r--    1 root     root           985 Sep  3 16:28 LogGen.sh
drwxr-xr-x    2 root     root           722 Jul  8 19:36 bin
drwxr-xr-x    5 root     root           920 Aug 18 18:07 dev
drwxrwxr-x    1 root     root             0 Jul  8 19:33 etc
drwxr-xr-x   11 root     root           807 Jul  8 19:26 lib
-rw-r--r--    1 root     root           136 Sep  3 16:29 log.txt
drwxr-xr-x    2 root     root             3 Jul  8 19:30 mnt
drwxr-xr-x    5 root     root             0 Jan  1  1970 overlay

```



We have sucessfully given ourselves (root) full permission.



So let's try to to run the LogGen.sh file without sh. 

```
root@Omega-24CF:/# ./LogGen.sh
-ash: ./LogGen.sh: Permission denied
root@Omega-24CF:/# chmod 755 LogGen.sh
root@Omega-24CF:/# ./LogGen.sh
Please Enter Your Name >Daisy
Donald Sat Sep 3 16:28:53 GMT 2016
Tick Sat Sep 3 16:29:01 GMT 2016
Trick Sat Sep 3 16:29:10 GMT 2016
Track Sat Sep 3 16:29:23 GMT 2016
Daisy Sat Sep 3 16:50:34 GMT 2016
root@Omega-24CF:/# 

```
