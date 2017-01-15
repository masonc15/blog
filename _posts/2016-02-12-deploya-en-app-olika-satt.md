---
layout: post
title: Deploya en app - Olika sätt
date: 2016-02-12 13:24:05.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _publicize_pending: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '19732090350'
  original_post_id: '756'
  _wp_old_slug: '756'
  _oembed_6859b230289a960a38d31ade1f52943d: "{{unknown}}"
  _oembed_bca3a7f40470e032fd3fd4bef209c93f: "{{unknown}}"
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
https://www.digitalocean.com/community/tutorials/how-to-set-up-automatic-deployment-with-git-with-a-vps
1. Logga in på VPS.
2. Skapa **git init --bare** i vps.
3. Gå till hooks, skapa filen post-receive
4. Lägg till följande

```bash
#!/bin/sh
git --work-tree=/var/www/domain.com --git-dir=/var/repo/site.git checkout -f
```

5. chmod +x post-receive
6. git remote add live ssh://user@mydomain.com/var/repo/site.git
7. git push live master
