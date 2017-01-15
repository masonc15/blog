---
layout: post
title: Bra css/jQuery/js trix
date: 2015-10-19 03:11:52.000000000 -03:00
type: post
published: true
status: publish
categories:
- css
- javascript
- jQuery
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '15966805468'
  original_post_id: '527'
  _wp_old_slug: '527'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

# Smooth-scroll till en sektion

Gör smooth-scroll till en sektion.
Lägg till den här koden i din .js-fil. Och ankra sedan länkarna till deras ID. Ex. Koden är copypastad [här](https://css-tricks.com/snippets/jquery/smooth-scrolling/).

```html
<section id="portfolio-section">
</section>
```

```javascript
$(function() {
  $('a[href*=#]:not([href=#])').click(function() {
    if (location.pathname.replace(/^//,'') == this.pathname.replace(/^//,'') && location.hostname == this.hostname) {
      var target = $(this.hash);
      target = target.length ? target : $('[name=' + this.hash.slice(1) +']');
      if (target.length) {
        $('html,body').animate({
          scrollTop: target.offset().top
        }, 1000);
        return false;
      }
    }
  });
});
```

# Fade in från vänster eller höger när man skrollar ner

[Här](http://jackonthe.net/css3animateit) finns filerna och dokumentationen.
