---
layout: post
title: How to solve Apt-get update error
date: 2016-04-05 17:14:42.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- apt
- error
- linux
- ubuntu
meta:
  _oembed_9b000067aa640e4ef4956ddfcdda3e52: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21483324526'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

So recently I have had some problems Advanced Package Tool, better known as APT. The popular linux package manager.
I have received the following errors when I have run:

```bash
sudo apt-get update
```

```bash
W: Failed to fetch http://archive.ubuntu.com/ubuntu/dists/trusty/Release Unable to find expected entry 'universe/source/Sources' in Release file (Wrong sources.list entry or malformed file)
```

I tried to regenerate the sources.list several times. By removing it and then running apt-get update again. I also read about a million posts on askubuntu and stack overflow.
In the end, the fix for me was to generate a new source.list. This is the tool I used to create a new source.list file.
[https://repogen.simplylinux.ch](https://repogen.simplylinux.ch/)
You then just copy-paste it and add it to /etc/apt/sources.list
Then I ran:

```
bashsudo apt-get update
```

But that threw a lot of errors. So I ran, as apt suggested,
```bash
sudo apt-get update --fix-missing
```

And that fixed the whole issue, at least for now.
