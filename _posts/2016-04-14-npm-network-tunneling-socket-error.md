---
layout: post
title: NPM "network tunneling socket"-error
date: 2016-04-14 14:04:28.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- error
- npm
- solution
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21780839821'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Sometimes when I have tried to download npm-packages I have received the following error:
```
npm ERR! node v4.3.1
npm ERR! npm  v2.14.12
npm ERR! code ECONNRESET
npm ERR! network tunneling socket could not be established, cause=connect ECONNREFUSED 127.0.0.1:8080
npm ERR! network This is most likely not a problem with npm itself
npm ERR! network and is related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'
npm ERR! Please include the following file with any support request:
```
Which is strange since I am not behind a proxy. I am not quite sure what the real issue here is. But I have found that a solution for it is:
```
npm config set proxy false
npm cache clean
```
This seems to do the trick for me.
