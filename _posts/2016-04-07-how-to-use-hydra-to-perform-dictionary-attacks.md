---
layout: post
title: How to use hydra to perform dictionary attacks
date: 2016-04-07 19:30:07.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- hydra
- security
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21558690705'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

So I have been playing around with some vulnerable VMs (from the awesome [vulnhub.com.](https://www.vulnhub.com) Some of them have had ftp and ssh services running on them. So I have tried to make dictionary attacks against them.
First we need some dictionaries with passwords. Here is a great [collection](https://github.com/danielmiessler/SecLists/tree/master/Passwords) of dictionaries/password-lists

This is the basic syntax. So first we add the list with usernames. Then the list of passwords. Then the ip. Then we specify the port `-s` then the service (in this case ssh). `-V` is for verbose mode.
The `-s` is only needed if the service is on another port than the default.

```
hydra -L userlist.txt -P best1050.txt 192.168.1.103 -s 45061 ssh -V
```

The man page is really quite useful.
