---
layout: post
title: Ettercap - MiTM
date: 2015-12-16 04:57:55.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _publicize_pending: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '17840222356'
  original_post_id: '680'
  _wp_old_slug: '680'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

1. Ladda ner och installera Kali linux.
2. Innan vi öppnar ettercap så måste vi göra vissa konfigureringar.
    - Öppna /etc/ettercap/ettercap.conf
    -  Scrolla ner till och ändra till följande

```
[privs]
ec_uid = 0 #65534
ec_gid = 0 #65534
```

Och kommentera sedan ut under sektionen redir_command_on/off.
Linux
kommentera ut
if you use ip-tables

```
redir_command_on = "iptables -t..."
redir_command_on = "iptables -t..."
```
3. Öppna upp Ettercap gui.
4. Klicka på "Sniff" och sedan på "Unified snffing", sedan väljer du nätverksinterface.
5. Klicka på Scan for hosts, under Hosts. Du kan behöva scanna fler gånger för att få upp alla maskiner som är uppkopplade mot nätverket.
6. Klicka sedan på routern (192.168.1.1) å sen target 1 sedan victim-datorn target 2. Om du inte väljer några targets så kommer alla att MiTMlas. Vilket kan vara påfrestande för nätverket.
7. Klicka på MiTM och sedan Arp poisining, och sedan kryssa i Sniff remote connections.
8. Klicka på Start, och sedan Start sniffing.
9. Nu kommer Ettercap att registrera alla användarnamn och lösenord som skrivs in. Givet att dom skrivs in i HTTP och inte i HTTPS.
Attacken är inte speciellt lyckad eftersom moderna webbrowsers säger till varje gång man besöker en https-sajt, den säger då till och varnar för att nån sniffar trafiken.
10. För att testa om det verkligen fungerar så kan du gå in på familjeliv.se och testa logga in. Du behöver inte ens logga in, utan bara skriva in ett fejkat användarnamn och lösen.
