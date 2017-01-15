---
layout: post
title: Vi/vim-kommandon
date: 2015-04-14 15:45:37.000000000 -03:00
type: post
published: true
status: publish
categories:
- how-to
- web-utveckling
- webbdesign
tags:
- vim
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  switch_like_status: '1'
  _publicize_job_id: '10855937008'
  original_post_id: '86'
  _wp_old_slug: '86'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Bra vi/vim-kommandon att använda. Här finns ett [](http://bullium.com/support/vim.html">cheatsheet.
## För att komma in i kommandoläge:
`ESC + :`

## För att komma in i insertläge:
`i`

## Kommandon:
```
w - spara
wq - spara och avsluta
q! - spara inte
## Flytta pekare
h - vänster
l - höger
j - upp
k - ner
längst ner i filen - G
längst upp i filen - gg
A - flyttar markören till sista karaktären i en rad.
I - flyttar markören till första karaktären i en rad.
## Ta bort
x - ta bort enskild tecken
dd - tar bort en hel rad
dG - tar bort allt nedanför markören.
```
## Redigera text
```
o - ny rad
```
## Fixa indentering

Detta är just nu mitt favoritkommando. Tänk dig att indenteringen är helt knasig och felaktig, istället för att för hand gå igenom å fixa det så kan du göra följande.
Först ställ markören längst upp på dokumentet (gg). Därefter skriv =G
Då fixas indenteringen automatiskt.

# Söka
## Sätt 1
### I kommandoläge:
<pre>/dinsökning</pre>
Klicka sedan på **n** för att se nästa sökresultat, och **N** (stor bokstav) för att se föregående sökresultat.
## Sätt 2
I kommandoläge placera markören på det ord du vill söka efter. Klicka sedan på *****, och du transporteras automatiskt till nästa tillfälle där det ordet använts.
# Kopiera
För att kopiera inom VIM.
1. Gå in i kommandoläga.
2. Placera markören där du vill började kopiera.
3. Klicka **v** och markera området som du vill kopiera.
4. Klicka **y** för att kopiera och **d** för att klippa ut.
5. Klicka **p** för att klista in.
**För att kopiera utanför vim.**
1. Markera texten du vill kopiera.
2. Klicka **"**+**y**
3. Gå till de dokument där du vill klistra in och kör **Crl+p**.
## Split window
Ibland kan det vara effektivt att ha två dokument uppe i samma fönster. Om man till exempel redigerar en css och html-dokument samtidigt.
För att öppna ett nytt dokument
**:split filename**
Till exempel:
**:split css/style.css**
För att göra så att fönstrena blir 50% vardera. Använd kommandot:
**ctr-w och snabbt däreftet =**
alltså **ctr-w =**
För att gå mellan fönster används kommandot**
ctr-w**
Här är en bra [](https://www.cs.oberlin.edu/~kuperman/help/vim/windows.html">guide för split window kommandon.
