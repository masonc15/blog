---
layout: post
title: Walkthrough SkyDog Con CTF – The Legend Begins
date: 2016-04-27 01:42:23.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- CTF
- pen-testing
- security
- vulnhub
- wargames
meta:
  _oembed_9dc51cac855cb2567a6b9add0a5c1b17: "{{unknown}}"
  _oembed_1cbc5c125eadeac55504e39cd1423631: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '22204581866'
  _oembed_70ee67d5f11d28e2a830edf536d701e0: "{{unknown}}"
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Okay, I wish I could say that I really solved this but I didn't get all the flags. But I am going to do a write up anyways, to not forget what I learned.
The CTF is called **SkyDog Con CTF – The Legend Begins**, and can be found https://www.vulnhub.com/entry/skydog-1,142/. Thanks [James Bower](http://www.jamesbower.com/) for a fun CTF!

### Instructions

```
The CTF is a virtual machine and works best in Virtual Box. This OVA was created using Virtual Box 4.3.32. Download the OVA file open up Virtual Box and then select File –> Import Appliance. Choose the OVA file from where you downloaded it. After importing the OVA file above it is best to disable the USB 2.0 setting before booting up the VM. The networking is setup for a NAT Network but you can change this before booting up depending on your networking setup. If you have any questions please send me a message on Twitter @jamesbower and I’ll be happy to help.
**Goal of Sky Dog Con CTF**
The purpose of this CTF is to find all six flags hidden throughout the server by hacking network and system services. This can be achieved without hacking the VM file itself.
**Flags**
The six flags are in the form of flag{MD5 Hash} such as flag{1a79a4d60de6718e8e5b326e338ae533
Flag #1 Home Sweet Home or (A Picture is Worth a Thousand Words)
Flag #2 When do Androids Learn to Walk?
Flag #3 Who Can You Trust?
Flag #4 Who Doesn’t Love a Good Cocktail Party?
Flag #5 Another Day at the Office
Flag #6 Little Black Box
```


### First flag
Let's search the network and scan the machine.

```
$ nmap -v 192.168.1.1/24                                                                                                [19:03:49]
Nmap scan report for 192.168.1.108
Host is up (0.0053s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
$ nmap -A -T4 -v -p- 192.168.1.108                                                                                      [19:06:08]
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 c8:f7:5b:33:8a:5a:0c:03:bb:6b:af:2d:a9:70:d3:01 (DSA)
|   2048 01:9f:dd:98:ba:be:de:22:4a:48:4b:be:8d:1a:47:f4 (RSA)
|_  256 f8:a9:65:a5:7c:50:1d:fd:71:57:92:38:8b:ee:8c:0a (ECDSA)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 252 disallowed entries (15 shown)
| /search /sdch /groups /catalogs /catalogues /news /nwshp
| /setnewsprefs? /index.html? /? /?hl=*& /?hl=*&*&gws_rd=ssl
|_/addurl/image? /mail/ /pagead/
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Okay, so we got ssh and port 80. Nmap also reveled that there's a lot of action in robots.
But first I want to check out port 80.
On the first page there is a image. I remember the name of the first flag: "Flag #1 Home Sweet Home or (A Picture is Worth a Thousand Words)". Okay, so I download the image and check it out in exiftool.

```
wget http://skydog.dev/SkyDogCon_CTF.jpg
exiftool SkyDogCon_CTF.jpg
```

And BAM first flag. Found in the comment.

```
XP Comment                      : flag{abc40a2d4e023b42bd1ff04891549ae2}
```

This is when I started getting cocky. If it is this easy, it's gonna be a breeze. Shame on me.
Now I check out the robots.txt file. And BOOM another flag.

```
# Congrats Mr. Bishop, your getting good - flag{cd4f10fcba234f0e8b2f60a490c306e6}
```

So in the robots file there was a lot of entries.
Many of them looked like this:

```
For example stuff like this
Allow: /?hl=*&gws_rd=ssl$
Disallow: /?hl=*&*&gws_rd=ssl
Allow: /?gws_rd=ssl$
Allow: /?pt1=true$
```

I was sure that this was meant for some sql-injections. So I fired up sqlmapping, but nothing.
So I figured that I would see which of all the pages worked, because most of them 404ed. So in order to do that in a efficient way (and inefficient, since nikto already told me which pages responded with 200) I figured that it would be fun to do it with bash.
There are probably a million ways to write this code in a better way. But it worked for me.
First I used cut to cut out all the urls and store them in a file I called robbo.

```bash
cut -d/ -f2-5 robots.txt > robbo
```

Then I wrote and ran this little script, which outputs the headers of the requests into the file output.

```bash
#!/bin/bash
while read p; do
  #echo $p
  echo http://skydog.dev/"$p" >> output
  curl --head http://skydog.dev/"$p" >> output
done <robbo
```

Then I ran grep on that file to show me all the 200s.

```bash
grep 200 -A 3 -B 3 output
```

So yeah, not very efficient. But it led me to this url: http://skydog.dev/Setec/
But that was not really thanks to my crappy script. I had found it when I used the spider in ZAP as well. Anyways, that page led me to this: http://skydog.dev/Setec/Astronomy/ where I found the zipfile Whistler.zip.
I downloaded it and tried to open it. But it required a password. So I started googeling and found fcrackzip. And I started playing around with it. But in the end I ran the wrong command

```
$ fcrackzip -D -p rockyou.txt Whistler.zip
possible pw found: yourmother ()
possible pw found: jinglebells ()
possible pw found: 200595 ()
possible pw found: spellman ()
possible pw found: jenny86 ()
possible pw found: julie10 ()
possible pw found: nascar7 ()
possible pw found: millie25 ()
possible pw found: hackett1 ()
possible pw found: chrebet ()
```

It just returned tens of possible passwords.
I should have run it like this:

```
fcrackzip -D -v -u -p rockyou.txt Whistler.zip
found file 'flag.txt', (size cp/uc     50/    38, flags 9, chk 874a)
found file 'QuesttoFindCosmo.txt', (size cp/uc     72/    61, flags 9, chk 83b5)
PASSWORD FOUND!!!!: pw == yourmother
```

Yeah I was stuck here and though that there was something wrong with the program or something. So I cheated a bit and learned the correct way to use fcrackzip.
I got the flag: flag{1871a3c1da602bf471d3d76cc60cdb9b}% and a clue for the next flag:
"Time to break out those binoculars and start doing some OSINT% "
So I started googeling about OSINT.
Osint stand for Open Source Intelligence. Something I didn't know of before reading about it. After reading about it on wikipedia I gather that it doesn't concern what programmers know of open source. It means more like public. Like public information gathering. It comes from the intelligence community.
Here I got really stuck again. And so I cheated. Again. Sorry.
I read in another walkthrough that he had taken out words from the movie sneakers imdb and ran it through dirbuster.
So I did that as well.
I took the movie script and downloaded it. Then I wrote the following bash-script:

```bash
#!/bin/bash
for word in $(<sound.txt)
do
    echo "$word" >> sneakersWord2.txt
done
```

It takes sound.txt as input and lines up each word in the file sneakers.txt. Which I then used in ZAP.
So I found the path:

```
/PlayTronics/
```

In /PlayTronics I got the flag:
http://skydog.dev/PlayTronics/flag.txt
And the next clue. http://skydog.dev/PlayTronics/companytraffic.pcap
A package capture of network traffic. So I ran:

```
tcpick -C -yP -r companytraffic.pcap > companytraffic.txt
```

And started poking around in it with grep. But yeah. I didn't really get anywhere with it. This is where I just gave up.
If you want to find the rest of the flags check out [g0blins great write-up](https://research.g0blin.co.uk/skydog-con-ctf-writeup) if you haven't already.

## Conclusion

I really made a lot of mistakes on this one, and some stuff was just over my head. Like somehow remaking the sound-clip from the pcap-file. That would have been cool to do.
I would have easily gotten the zip-file if I just had learned the tool a bit better.
I should also have read the instructions better! If I had done that I would have figured out that I of course should have tried to crack the MD5 hashes.
I got to play around with some more unix commands like cut, and writing a bit in bash which is always useful. I also got to try out fcrackzip, although I doubt it will ever be useful. It seems like a really old technology.
So all in all a fun CTF and I learned some more. Which in the end is the most important thing.
