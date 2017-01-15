---
layout: post
title: Skapa ett grid-system enligt Bootstrap-modellen
date: 2015-05-21 16:41:45.000000000 -03:00
type: post
published: true
status: publish
categories:
- bootstrap
- css
- web-utveckling
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '10857592354'
  original_post_id: '303'
  _wp_old_slug: '303'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Den bästa förklaringen för hur ett grid-system bör byggas är av [Udacity](https://www.udacity.com/course/viewer#!/c-ud304/l-2810388540/e-2872198559/m-2870208566).

Det är inte helt enkelt att positionera element på en sida. Det bästa sättet för att få tydligt kontroll över elementens position är att skapa ett grid. Alltså ett nät av kolumner. Det vanligaste och bästa sättet är att skapa ett grid med tolv kolumner. Eftersom tolv är ett nummer som kan delas på många olika sätt 12/6, 12/4, 12/3, 12/2.
För att göra gridet responsivt bör det skrivas i procent och inte i pixel.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Projekt 2</title>
        <link rel="stylesheet" href="css/style.css">
    </head>
    <body>
        # Framework test page
        <div class="grid">
            <div class="row">
                <div class="col-2">Two columns</div>
                <div class="col-10">Ten columns</div>
            </div>
            <div class="row">
                <div class="col-3">Three columns</div>
                <div class="col-9">Nine columns</div>
            </div>
            <div class="row">
                <div class="col-4">Four columns</div>
                <div class="col-8">Eight columns</div>
            </div>
            <div class="row">
                <div class="col-6">Six columns</div>
                <div class="col-6">Six columns</div>
            </div>
            <div class="row">
                <div class="col-7">Seven columns</div>
                <div class="col-5">Five columns</div>
            </div>
            <div class="row">
                <div class="col-8">Eight columns</div>
                <div class="col-4">Four columns</div>
            </div>
        </div>
    </body>
</html>
```

Och här är CSS

```css
* {
    border: 1px solid red !important;
}
* {
    box-sizing: border-box;
}
.grid {
    margin: 0 auto;
}
.row {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
}
.col-1 {
    width: 8.33%;
}
.col-2 {
    width: 16.66%;
}
.col-3 {
    width: 25%;
}
.col-4 {
    width: 33.33%;
}
.col-5 {
    width: 41.66%;
}
.col-6 {
    width: 50%;
}
.col-7 {
    width: 58.33%;
}
.col-8 {
    width: 66.66%;
}
.col-9 {
    width: 74.99%;
}
.col-10 {
    width: 83.33%;
}
.col-11 {
    width: 91.66px;
}
.col-12 {
    width: 100%;
}
h1 {
    color: green;
}
```

Så gird-systemet består helt enkelt av en row och kolumner. Kolumnerna kan implementeras på många olika sätt. För att divvarna ska hamna bredvid varandra, och inte under varandra behöver man lägga till attributet `display` i elementet `.row`, och ge det värdet: `flex; wrap-flex: wrap;`. Se koden ovan.
