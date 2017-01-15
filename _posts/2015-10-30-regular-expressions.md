---
layout: post
title: Regular expressions
date: 2015-10-30 23:02:58.000000000 -03:00
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
  _publicize_job_id: '16378228696'
  original_post_id: '545'
  _wp_old_slug: '545'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

**^ och $**
`^` betyder att texten som söks endast får finnas i början av en rad.
`$` betyder att texten som söks endast får finnas i slutet av en rad.
`^hej$` hittar endast hej om hej står själv på en egen rad.

**[Character classes]**

Om tecken är inom klamrarna så betyder det: vilket som.
`[ab]` betyder att både a och b söks.
så `gr[ea]y` kommer att hitta både gray och grey.
Inom en character class så är ett bindestreck - endast ett bindestreck, och ingen metakaraktär.
Inom character class (alltså inom `[]`) är betyder `^` negation. Alltså
`[^6]` betyder alla karaktärer som inte är 6
Du kan tänka på Character Classes som ett subspråk inom regex. Där finns det egna regler för hur de olika symbplerna ska tänkas.

**Alternatation**

Ibland vill man söka this or that. Det ena eller det andra. det kan man enkelt göra med hjälp av parenteser `och | or`.
`gr(a|e)y`
`|` betyder här OR. så antingen a eller e.
Så detta problem kan alltså antingen lösa med hjälp av alternation `()` eller Character Classes `[]`. Det finns dock en viktig skillnad. En character class kan endast skilja enskilda tecken åt, men med alternation så kan man skila flera bokstäver som sitter samman åt.
`(Bob|Robert)` kommer söka efter båda.
Till skillnad från Character Classes så har inte Alternation sitt eget subspråk, utan följer de vanliga reglerna för regex.

**Endast ord som inte finns inom andra ord**

Om jag vill hitta ordet `you`, men inte när det är inom `youtube`. Då kan man sätta upp word-boundries.
såhär: byoub

**Antal gånger**

`{4}` - betyder antalet gånger något skall försekomma. exempel:

```javascript
/[0-9]{4}/gi
//Betyder 0345 eller 5336. Alltså fyra nummer.
```

`{3}` Exactly 3 occurrences;
`{6,}` At least 6 occurrences;
`{1,5}` Mellan en till 5 gånger.
Ett annat sätt är att använda plustecknet.

`+` - betyder att karaktären innan ska repitera 1-oändligt antal gånger.
`/y+/` fångar yyyyyyy
