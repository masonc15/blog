---
layout: post
title: How to automatically send a notification through email when someone logs in
  through SSH to server
date: 2016-03-31 21:40:55.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21325750095'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Wow, that was a pretty log title.
I have been working a bit on setting up vulnerable servers to play CTF with some friends. So I wanted to have a system where the server emails me every time someone successfully logs in to the server.
It requires two steps.
1. Setting up email on the server
2. Writing script that sends email upon SSH-login

## Setting up email on the server
Email is sent through the [Simple Mail Transfer Protocol](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol). To make use of this protocol we need the program Simple SMTP(sSMTP).
In order to send an email directly from the server you need a domain-name. But I don't have time for that, so instead I am going to relay the email through an account at gmail.com. From what I have seen online, this seems to be the most common solution for small projects like this.
So let's download and install ssmtp.

```bash
sudo apt-get update
sudo apt-get install ssmtp
```

After we have installed ssmtp we need to do a bit of configuration.

```bash
sudo vim /etc/ssmtp/ssmtp.conf
```

```bash
# Config file for sSMTP sendmail
#
# The person who gets all mail for userids < 1000
# Make this empty to disable rewriting.
#root=postmaster
root=myEmailAddress@gmail.com
# The place where the mail goes. The actual machine name is required no
# MX records are consulted. Commonly mailhosts are named mail.domain.com
#mailhub=mail
mailhub=smtp.gmail.com:587
AuthUser=myEmailAddress@gmail.com
AuthPass=MyGmailPassword
UseTLS=YES
UseSTARTTLS=YES
# Where will the mail seem to come from?
#rewriteDomain=
rewriteDomain=gmail.com
# The full hostname
#hostname=MyMediaServer.home
hostname=localhost
# Are users allowed to set their own From: address?
# YES - Allow the user to specify their own From: address
# NO - Use the system generated From: address
FromLineOverride=YES
```

Wow easy-peasy, now let's send an email.

```bash
ssmtp myEmailAddress@gmail.com
To: blabla@gmail.com
From: myEmailAddress@gmail.com
Subject: Test
#ctr-d to send
```

Of course, that didn't work.
So i checked the log-file: `cat /var/log/mail.log`
Where I found this error:

```
Mar 31 17:13:25 ctf sSMTP[19275]: Authorization failed (534 5.7.14  https://support.google.com/mail/answer/78754 j8sm4704154qhj.19 - gsmtp)
```

So I started googeling it, and [this answer](http://serverfault.com/questions/635139/how-to-fix-send-mail-authorization-failed-534-5-7-14) lead me to this [answer from google](https://support.google.com/accounts/answer/6009563). That answer led me to this page: [https://g.co/allowaccess](https://g.co/allowaccess), which lets you allow access from other apps.
And now it is working.
Okay. So now any user on the server can read the config file (`/etc/ssmtp/ssmtp.conf`). Which includes out password, so that it not optimal. So let's set the file permission so that only root can read the file.

```bash
chmod 700 /etc/ssmtp/ssmtp.conf
```
Now you can check to see if other users can read the file or not.

```bash
#Granted that you are root
su userName
cat /etc/ssmtp/ssmtp.conf
#should output: cat: /etc/ssmtp/ssmtp.conf: Permission denied
```

There are many other email clients  out there. Mailx, mutt and sendmail are some.

## Check who is logging in
So how do we know if someone has logged in through ssh to out server?
My initial though was to parse the ssh-logfile and then run a cron-job that would check it every 10 minutes or so. But after some googleing I soon discovered that there is a much better way to solve the problem. [This SO-answer](http://askubuntu.com/questions/179889/how-do-i-set-up-an-email-alert-when-a-ssh-login-is-successful) provided a great solution.
First we create the bash-script and put it in `/etc/ssh/login-notify.sh`. This script is pretty straight-forward. We set the sender and recipient in each variable. And then we have an if-statement that returns true if anything except close_session happens. And then we use mailx to send the email.

```bash
#!/bin/sh
# Change these two lines:
sender="sender-address@example.com"
recipient="notify-address@example.org"
if [ "$PAM_TYPE" != "close_session" ]; then
    host="`hostname`"
    subject="SSH Login: $PAM_USER from $PAM_RHOST on $host"
    # Message to send, e.g. the current environment variables.
    message="`env`"
    echo "$message" | mailx -r "$sender" -s "$subject" "$recipient"
fi
```
Then we need to make it executable:
```bash
chmod +x /etc/ssh/login-notify.sh
```
You then add the following line the file: /etc/pam.d/sshd
```bash
#/etc/pam.d/sshd
session optional pam_exec.so seteuid /etc/ssh/login-notify.sh
```

Notice that is says:

```bash
# SELinux needs to be the first session rule.  This ensures that any
# lingering context has been cleared.  Without this it is possible that a
# module could execute code in the wrong domain.
```
So it is probably a good idea to add the above code after this paragraph.
So, who the hell is PAM? Well, PAM stands for Pluggable Authentication Modules, and is basically the program in charge of stuff that regards authentication. If we check:

```bash
ls /etc/pam.d
```

These are the config-files for the programs that uses pam. chsh, cron, newuser, passwd, login, sshd, and some others. Here you can really configure these programs down to details. For more about PAM check out [this excellent](http://www.tuxradar.com/content/how-pam-works) resource.
