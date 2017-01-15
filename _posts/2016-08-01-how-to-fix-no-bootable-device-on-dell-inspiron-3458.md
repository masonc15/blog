---
layout: post
title: 'How to fix: "No bootable device" on Dell Inspiron 3458'
date: 2016-08-01 01:59:10.000000000 -04:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _edit_last: '1959580'
  geo_public: '0'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '25348544777'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

I bought a new disk, a SSD. I am using a Dell Inspiron 3458. So I open up the computer and changed the disk, no problem so far. Really easy process. Then I rebooted and installed the OS, still no problem. But then when I tried to boot up the machine again a got hit with a scary error.
I can't remember exactly what it said, but it was something like this
```
No bootable device

Press F2 to enter bios-configuration

Press F5 to reboot
```

And some other stuff.
I was lost. So I started the great google-hunt. Read through a ton of blog-posts and videos, and everything I could get my hands on. I tried to change the boot-order to legacy mode. But nothing worked. In the system info in bios I could see that the machine recognized the disk, it just couldn't find it when I booted up.
Then I found this blog-post: https://itsfoss.com/no-bootable-device-found-ubuntu/. But it was written for an Acer Aspire. But I felt like this was the same issue as I had. And in the comments I found my anonymous hero Chris, who wrote the following comment:

```
I have a Dell Inspiron, I followed your process, more-or-less, but from step 3 (“Select an UEFI file as trusted for executing”) onwards I had to do something slightly different in the BIOS:

– Settings → General → Boot Sequence
– Press “Add Boot Option”
– Boot Option Name: ubuntu (I don’t think this matters much, but this seems like a sensible name)
– File Name: press the browse button, and as in the instructions above, find the shimx64.efi boot file
– Save this boot option
– Exit the BIOS, and with any luck your computer will now boot
```

I followed these steps and it worked perfectly.
