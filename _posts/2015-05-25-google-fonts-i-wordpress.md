---
layout: post
title: Google Fonts i Wordpress
date: 2015-05-25 21:16:38.000000000 -03:00
type: post
published: true
status: publish
categories:
- wordpress
tags:
- google font
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '11005380471'
  original_post_id: '342'
  _wp_old_slug: '342'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
För att google fonts ska funka väl i wordpress så måste du lägga till den som en funktion i din functions.php-fil.
Gå in i google fonts, leta reda på den du vill ha. Ta adressen som du ska använda i länk-taggen. Å klistra in länken här istället.
```php
<?php
function load_fonts() {
            wp_register_style('et-googleFonts', 'http://fonts.googleapis.com/css?family=Open+Sans:300italic,400italic,700italic,400,700,300');
            wp_enqueue_style( 'et-googleFonts');
        }
    add_action('wp_print_styles', 'load_fonts');
?>
```
Lägg in adressen där adressen är.
Sen får du lägga till fonten som font-family i din css.
