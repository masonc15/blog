---
layout: post
title: Bootstrap-klasser att känna till
date: 2015-05-24 17:00:16.000000000 -03:00
type: post
published: true
status: publish
categories:
- bootstrap
- web-utveckling
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '10964176884'
  original_post_id: '329'
  _wp_old_slug: '329'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Vissa klasser för bootstrap måste man helt enkelt memorera för att skynda på arbetet. Här är några.
## Bilder

```html
<img src="bild.jpg" class="img-responsive">
```
Det är inte mer komplicerat än att bilden blir responsiv. För mer info om klassen se [bootstraps](http://getbootstrap.com/css/#images) dokumentation.

### Centrera bild
Om du vill centera bilden så använd `.center-block` iställer för `.text-center`.

## Text

Om du vill positionerna texten så anväd dessa klasser.

```html
<p class="text-left">Left aligned text.
<p class="text-center">Center aligned text.
<p class="text-right">Right aligned text.
<p class="text-justify">Justified text.
<p class="text-nowrap">No wrap text.
```

Om du vill göra en viss text med stora bokstäver eller bara små använd följande klasser.

```html
<p class="text-lowercase">Lowercased text.
<p class="text-uppercase">Uppercased text.
<p class="text-capitalize">Capitalized text.
```
