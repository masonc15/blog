---
layout: post
title: The Document Object Model (DOM)
date: 2015-05-16 18:05:40.000000000 -03:00
type: post
published: true
status: publish
categories:
- web-utveckling
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  original_post_id: '244'
  _wp_old_slug: '244'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
DOM är en model över hur olika objekt i ett dokument förhåller sig till varandra.
Varje objekt är en node. Här är ett exempel

```html
Här är text
```

Den här koden består av två noder. Ett element (p) och en text-node (Här är text). Text-noden anses vara child-node i förhållande till (p) eftersom den är nested inom p.

```html
Här är **text**
```

I denna kod har p fått ett till child, nämligen (strong). Strong har i sin tur fått ett barn "text".
Sen finns det attribute-node. Som till exempel:

```html
<p ALIGN="right">Här är **text**
```

`ALIGN` är här ett tredje exempel på en node.
De tre vanligaste noderna är alltså **element-node**, **text-note** och
**attribute-node**, även whitespace mellan taggar är text-noder. Kan vara bra att känna till.

Så hela html-dokumentet kan förstås som ett träd.
Att förstå hur dessa noder hänger ihop är viktigt för att kunna manipulera dem, något som oftast görs med hjälp av JavaScript.
För mer om detta. [Länk](http://www.quirksmode.org/dom/intro.html).
En [bra video](https://www.youtube.com/watch?v=TmQ_v5LsakQ) som förklarar DOM.
