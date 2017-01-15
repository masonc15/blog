---
layout: post
title: Ta bort funktion i child-theme
date: 2015-04-23 19:13:04.000000000 -03:00
type: post
published: true
status: publish
categories:
- how-to
- web-utveckling
- wordpress
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  original_post_id: '128'
  _wp_old_slug: '128'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
1. CSS
Ett sätt. Antagligen det vanligaste är helt enkelt att gå in i style.css och skriva ut, som exempel:

```css
.author.vcard {
display: none;
}
```

Vilken kod som skall ändras hittas enklast genom att använda firefox-tillägget firebug som hittar css-taggarna hos olika element på hemsidor.
Negativa: Skripten körs fortfarande. Så hemsidan går inte nödvändigtvis snabbare, bara för att man tar bort CSS.
2. Importera filen och sedan ändra den.
Negativa: Man får inte tillgång till tema-uppdateringarna.
3. Skriva ut funktioner i `functiones.php`. [Här](http://code.tutsplus.com/tutorials/how-to-modify-the-parent-theme-behavior-within-the-child-theme--wp-31006) finns en bra förklaring. Filter å sånt.
Fördel: DÅ kan man uppdatera temat utan att behöva göra om ändringarna.
