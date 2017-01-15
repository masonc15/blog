---
layout: post
title: How to install and setup Apache/php/Mysql
date: 2016-04-02 22:14:16.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21391632376'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

## Installing Apache
So first we are going to install Apache. Which is really easy.

```bash
sudo apt-get update
sudo apt-get install apache2
```

Yeah, it is that easy. I am running the server on a VM (that I have configured to Bridged Adapter i the network-settings) so that it can connect to my network.
Now I can reach the initial page create by apache at the address of the servers internal ip (that looks something like this: 192.168.1.101). The page we see is just a "Your server is working"-page by apache. You can find it and remove it, or change it in `/var/www/html`.
But it could be a bit annoying having to write out the internal IP-address every time we want to visit the servers page. So we can give it a name by changing the config file in `/etc/hosts` on the host-computer.

```bash
sudo vim /etc/hosts
#Here we just add the internal IP-address of the server, and then we make a tab (now several spaces) and then the name. Like this:
192.168.1.101    mylocalhomepage.dev
```

Now we only need to restart the network service

```bash
sudo /etc/init.d/networking restart
```

And now we can view the apache-page on mylocalhomepage.dev
To develop with apache it can be quite annoying to always have to write sudo before writing in any file in /var/www/html.
There are two ways to solve this.
1. Change the permission of the dir /var/www/html.
2. Change the root-directory. So that apache looks elsewhere. Let's do that.
Go to:

```bash
#Let's first make a backup in case we mess up somewhow.
sudo cp 000-default.conf 000-default.conf.backup
#Then we edit the file to the following:
sudo vim /etc/apache2/sites-available/000-default.conf
DocumentRoot /home/yourUsername/www
```

This is a really strange name of the conf-file to me, but whatever. Now we need to change one more file and we are good to go.

```bash
#Make a copy. I like to have the original to look at if something happens.
sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.backup
sudo vim /etc/apache2/apache2.conf
#Change this part
<Directory /home/yourUser/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
#Then we restart apache so the changes take effect
sudo service apache2 restart

```
Now you can create a file in `/home/yourUser/www` called `index.html` and you will see it if you go to localhost.
I managed to mess something up and the googeling solution for a long time.

## Installing PHP5

```bash
sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt
```

Now you should be able to use apache as your server and render php-files. Create a file called index.php in your apache-root folder.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>PHP-page</title>
  </head>
  <body>
  <?php
    echo "test if php is working";
  ?>
  </body>
</html>
```

The php-code should be rendered on the serverside and output "test if php is working". If the code is put in a html-comment, it means that php is not working and active. And you will have to troubleshoot it.

## MySQL
Soon!
