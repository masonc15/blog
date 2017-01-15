---
layout: post
title: OOP - Javascript
date: 2015-10-25 22:06:02.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '16201338354'
  original_post_id: '535'
  _wp_old_slug: '535'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

# Fundamentals av OOP-Javascript
Det finns en skillnad mellan primitiv och object-data.
Såhär fungerar primitiva data.

```javascript
var test1 = 0;
var test2 = test1;
console.log(test2);//loggar 0
test2 = 10;
console.log(test2);//loggar 10
```

Såhär fungerar object-data

```javascript
var test1 = {
a: 0};
var test2 = test1;
console.log(test2.a);//loggar 0
test2.a = 10;
console.log(test1, test2);//båda loggar 10
```

Som vi kan se i det här exemplet så hänger alltså "kopian" av objektet ihop med orginalet, så om du ändrar kopian så ändras orginalet.
