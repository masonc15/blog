---
layout: post
title: tmux och irssi
date: 2015-12-23 00:08:46.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _publicize_pending: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '18044747863'
  original_post_id: '714'
  _wp_old_slug: '714'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

## Tmux
För att kunna kunna idla på irc 24/7 behöver man en server som jämnt är online, irssi och tmux.
Logga in på serverns med ssh.
Ladda ner irssi och tmux.

```
sudo apt-get install irssi tmux
```

Starta en ny session med tmux genom kommandot:
```
tmux
```
Öppna irc med kommandot:
```
irssi
```
joina en server och en kanal.
```
/connect irc.freenode.net
/join #namnpåkanal
```
För att sedan kunna gå ur å komma in igen å läsa all historik så kör kommandot:
```
ctrl a
ctrl d
crt-b D
```
För att sedan gå ut ur alla tmux-sessioner så kör du.
```
ctrl-d
För att sedan hoppa in på en ny session igen så kör du:
```
tmux list-sessions
tmux attach -t 0 (eller vad sessionen nu heter)
ctr-b p - för att gå över till nästa panel
ctr-b , - för att renamea ett fönster
```
