---
layout: post
title: Setting up SSH to Ubuntu-server
date: 2016-04-01 20:51:11.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21359446215'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

I am running Ubuntu Server on a VM, but you can't copypasta commands and stuff to it (I have set the bidirectional settings in Virtual Box - but it only works in gui-ubuntu I think). So the next best thing is just to SSH into the VM. It's pretty much the same thing but I get to use the Terminal instead of the crappy terminal that is on the server.

But I always forget the process of setting up SSH. So here it is.
On the VM we first need to go into the settings in VirtualBox. To network and then set Bridged Adapter. This allows the VM to obtain an internal IP and connect it to the rest of the network.

```bash
sudo apt-get update
sudo apt-get install openssh-server
sudo ufw allow 22
```

Updating, downloading and installing openssh-server. And then we open ssh port 22 on the firewall.
Now we need to know out servers internal IP so that we can connect to it. There are a few ways to find it.

```bash
ifconfig
ip addr show
#or
ip route get 8.8.8.8 | awk '{print $NF; exit}'
```

Either way the address should look something like this: 192.168.1.101
Now let's log in:

```bash
ssh username@192.168.1.101
```

Now we get this:

```bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
21:01:12:c2:6c:01:51:0b:ef:19:76:79:d2:87:d0:13.
Please contact your system administrator.
Add correct host key in /home/user/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /home/user/.ssh/known_hosts:11
  remove with: ssh-keygen -f "/home/user/.ssh/known_hosts" -R 192.168.1.101
Password authentication is disabled to avoid man-in-the-middle attacks.
Keyboard-interactive authentication is disabled to avoid man-in-the-middle attacks.
Permission denied (publickey,password).
```

Okay so that didn't work. So let's run the command that the error-message suggest.

```bash
ssh-keygen -f "/home/user/.ssh/known_hosts" -R 192.168.1.101
```
And now we can log in without any problems.
