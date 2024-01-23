# iocage-plugin-piwigo

Script to create a TrueNAS jail and install [Piwigo](http://piwigo.org) photo management software. 

*I have tested this plugin a couple of times on my TrueNAS 13 U6 as a **13.2-RELEASE**, all seems to work well.*

![Piwigo Installation Successful](https://piwigo.org/plugins/piwigo-piwigodotorg//images/home/responsive-screens.png)

Tip 1. Please remember to read info in **TrueNAS / Plugins / Piwigo / POST INSTALL NOTES** - to access info with DB user and DB password.
Piwigo first installation page will set Host: as localhost, usually this needs to be changed to 127.0.0.1
>   Host: 127.0.0.1

Tip 2. Please set your own date/time location in PHP.INI, as for this installation I have chosen Europe/London ;)

To install Piwigo plugin manually from the internet:

> iocage fetch -P piwigo -g https://github.com/rchampagneca/iocage-plugin-piwigo -r 13.2-RELEASE ip4_addr="interface|IPaddress"

where *interface* is the name of the active network interface and *IP address* is the desired IP address for the plugin. For example, ip4_addr="igb0|192.168.0.91"

## Caddy

This script uses the Caddy web server, which supports automatic HTTPS, reverse proxying, and many other powerful features. It is configured using a Caddyfile, which is stored at /usr/local/www/Caddyfile in your jail, and under /apps/piwigo/ on your main data pool. You can edit it as desired to enable these or other features. For further information, see the [Caddy docs](https://caddyserver.com/docs/caddyfile).

## Post installation settings
<h6> PHP

```
    date.timezone = "Europe/London"
    max_execution_time = 300
    max_input_time = 300
    post_max_size = 100M
    upload_max_filesize = 100M
    memory_limit = 512M
```
Enable this extension for mysqli in PHP.ini

```
    extension=mysqli
```

## Update... 9/1/2024

I successfully updated my iocage Release 12.2 to 13.1, this is my procedure:
<h6>outside the iocage

```
iocage upgrade -r 13.1 piwigo

Note: This will take some time
- Type "y" for yes when inquiries occur
- Type "q" for "quit" when the validation list appears, you may need to type it several times depending on the number of affected items
- Type "y" for yes for any additional inquiries
```

<h6>inside the iocage

```
pkg update && pkg upgrade -y

pkg install php73 php73-session php73-mysqli nginx mariadb105-server ImageMagick7-nox11 git php73-exif php73-filter php73-gd php73-mbstring php73-zip php73-zlib php73-pecl-json_post finfo php73-fileinfo
```
Than restart iocage and log in to your gallery. I had an option to upgrade Piwigo to 14.1.0 ;)

