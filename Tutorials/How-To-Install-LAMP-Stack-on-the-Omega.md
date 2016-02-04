# How To Install Linux, Apache, MySQL, PHP (LAMP) Stack on the Omega

LAMP stack is a group of open source software used to get web servers up and running. The acronym stands for Linux, Apache, MySQL and PHP. Since the Omega itself runs OpenWRT, which is a Linux operating system, the linux part is taken care of. Here's how we install the rest.

## Step 1. Expand your storage with USB

Apache as other packages requires a lot of space so first thing You will have to do is to extend your storage with extroot. More information about how to do that can be found at [[Tutorials/Using-USB-Storage-as-Rootfs]].

## Step 2. Setting Up Apache

```
opkg update
opkg install apache
```

Then edit `/etc/apache/httpd.conf`:

Find the line that starts with `Listen` and change it to:

```
Listen <ip You use to connect to Your console>:<port number other than 80>
```

Also find the line that starts with `ServerName` and change it to:

```
ServerName <name for Your server>:<same port as in Listen>
```

Then you should find `LogLevel debug`, and change it into:

```
LogLevel error
```

Now we are ready to start the Apache server:

```
apachectl start
```

Your webserver can be accessed at: `<Onion Console IP adress>:<Your chosen port>`.


## Step 3. Setting Up PHP

```
opkg install php5 php5-cgi php5-fastcgi
```

Next, we will create a symbolic link to allow PHP to handle CGI requests:

```
ln -s /usr/bin/php-cgi /usr/share/cgi-bin/
```

We will also need to tell Apache where to send all the CGI requests. To do this, we add the following lines to `/etc/apache/httpd.conf`:

```
<Directory "/usr/share/cgi-bin">
    AllowOverride None
    Options FollowSymLinks
    Order allow,deny
    Allow from all
</Directory>

<Directory "/usr/bin">
    AllowOverride None
    Options none
    Order allow,deny
    Allow from all
</Directory>
```

Find the line `ScriptAlias /cgi-bin/ "/usr/share/cgi-bin"` and add `ScriptAlias /php/ "/usr/bin"` after it:

```
ScriptAlias /cgi-bin/ "/usr/share/cgi-bin"
ScriptAlias /php/ "/usr/bin/"
```

Find `<IfModule mime_module>` section and add the following:

```
AddType application/x-httpd-php .php
Action application/x-httpd-php "/php/php-cgi"'
```

To change the Directory Index, find `DirectoryIndex index.html`, and change change it to the following:

```
DirectoryIndex index.php index.html
```

Next, open up `/etc/php.ini` and uncomment (or remove) the following lines:

```
extension  = gd.so
extension = mbstring.so
extension = pdo-mysql.so
extension = pdo_sqlite.so
extension = socket.so
```

Also do not specify `doc_root` so change `doc_root` into `doc_root =`

After that restart the Apache server to reload the changed configurations:

```
apachectl restart
```

## Step 4. Setting Up MySQL

```
opkg install mysql-server php5-mod-mysqli
```

Create two folders for MySQL:

```
mkdir -p /mnt/data/mysql
mkdir -p /mnt/data/tmp
```

Once you have installed MySQL, we should activate it with this command:

```
mysql_install_db --force
```

Next step is to create root user (when I installed mysql, table with users was empty, if that's also Your case use this steps). Execute:

```
/usr/bin/mysqld --skip-grant-tables --skip-networking &
mysql -u root
use mysql;
FLUSH PRIVILEGES;
INSERT into user (Host, User,Password) values ('localhost','root',' ');
UPDATE user SET Password=PASSWORD('password you want') WHERE User='root';

update user set Select_priv='Y',Insert_priv='Y',Update_priv='Y',Delete_priv='Y',Create_priv='Y',Drop_priv='Y',Reload_priv='Y',Shutdown_priv='Y',Process_priv='Y',File_priv='Y',Grant_priv='Y',References_priv='Y',Index_priv='Y',Alter_priv='Y',Show_db_priv='Y',Super_priv='Y',Create_tmp_table_priv='Y',Lock_tables_priv='Y',Execute_priv='Y',Repl_slave_priv='Y',Repl_client_priv='Y',Create_view_priv='Y',Show_view_priv='Y',Create_routine_priv='Y',Alter_routine_priv='Y',Create_user_priv='Y' where user='root';

FLUSH PRIVILEGES;
```

To start the MySQL server you should type:

```
/etc/init.d/mysqld start
```

And that's it :)

Happy Hacking!

## Credits

Thanks for [Josip Mlakar](https://community.onion.io/user/josip-mlakar) for writing this awesome tutorial!
