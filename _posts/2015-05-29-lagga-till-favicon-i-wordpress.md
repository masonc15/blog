---
layout: post
title: Lägga till Favicon i Wordpress
date: 2015-05-29 03:41:27.000000000 -03:00
type: post
published: true
status: publish
categories:
- web-utveckling
- wordpress
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '11118069625'
  original_post_id: '346'
  _wp_old_slug: '346'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Favicon är den lilla ikonen bredvid titeln på hemsidan. För att lägga till en sådan ikon i Wordpress gör du bara följande.
Först skapar du en ikon. Om du vill omvandla den till .ico så kan du göra det via den [här hemsidan](http://tools.dynamicdrive.com/favicon/). Sedan laddar du upp ikonen i root-mappen på din wordpressinstallation. Alltså under `/wp-content/`. Därefter lägger du till följande kod i din `header.php`.

```html
<link rel="icon" href="<?php bloginfo('siteurl'); ?>/favicon.ico" type="image/x-icon" />
<link rel="shortcut icon" href="<?php bloginfo('siteurl'); ?>/favicon.ico" type="image/x-icon" />
</head> <!--Här slutar din head -->
```
Sådär, då var det klart.
