---
layout: post
title: Nmap commands
date: 2016-04-07 14:42:09.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- nmap
- security
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21550400670'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

## Find hosts/devices on the network

```
nmap -sP 192.168.0.1/24
```

To find out what devices are on the network you need to know the ip of the router.
Here are some common:
```
10.0.1.1
10.0.0.2
10.0.0.138
192.168.0.1
192.168.1.1
192.168.1.10.1
192.168.11.1
192.168.2.1
192.168.3.1
192.168.1.254
192.168.254.254
```
[Here](http://www.techspot.com/guides/287-default-router-ip-addresses/) is a more complete list, and according to model.
For default usernames and passwords check [here.](http://www.routeripaddress.com/)

## Scan for open ports

The most simple of commands to check for some standard ports is:

```bash
nmap 192.168.0.103
```

But that does not check all of the possible ports.

```bash
nmap -p 1-65535 192.168.0.164
```

Or decide what ports you do want to check.

## Check what service uses a specific port
Let's say we find some open ports. But the port is either to high to have a specific service. Or it is on a port that is not usually used for it. For example, a lot of people are moving their ssh to not be port 22 to avoid spam-attacks.

```bash
45061/tcp open  unknown
```

So we just add the -sV flag.

```
sudo nmap -sV -p 45061 192.168.1.103
```

Here is the [Nmap-documentation](https://nmap.org/book/man-version-detection.html) for it.
