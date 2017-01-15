---
layout: post
title: Lägga in javascript-script i Wordpress parentheme
date: 2015-09-01 15:11:25.000000000 -03:00
type: post
published: true
status: publish
categories:
- javascript
- jQuery
- wordpress
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '14321360501'
  original_post_id: '454'
  _wp_old_slug: '454'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Leta efter sektionen i functions.php där script läggs till. Det kan se ut såhär.

```php
function resonate_scripts() {
	wp_enqueue_style( 'resonate-style', get_stylesheet_uri() );
	wp_enqueue_script( 'resonate-navigation', get_template_directory_uri() . '/js/navigation.js', array(), '20120206', true );
	wp_enqueue_script( 'resonate-skip-link-focus-fix', get_template_directory_uri() . '/js/skip-link-focus-fix.js', array(), '20130115', true );
/* Här är mitt eget script so jag har lagt till. Det är baserat på jQuery det är därför som jquery finns i arrayen */
	wp_enqueue_script(
		'animations',
		get_stylesheet_directory_uri() . '/js/script.js',
		array( 'jquery' )
	);
	if ( is_singular() && comments_open() && get_option( 'thread_comments' ) ) {
		wp_enqueue_script( 'comment-reply' );
	}
}
add_action( 'wp_enqueue_scripts', 'resonate_scripts' );
```
