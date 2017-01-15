---
layout: post
title: Hur man lägger till javascript-fil in en wordpress child
date: 2015-08-06 15:17:04.000000000 -03:00
type: post
published: true
status: publish
categories:
- jQuery
- wordpress
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '13465193745'
  original_post_id: '417'
  _wp_old_slug: '417'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Wordpress har redan jQuery inbyggt, så du behöver bara lägga till ditt script.  
1. Gå till child-temat och gå in i filen `functions.php`.
Lägg där till följande kod.

```php
function add_my_script() {
    wp_enqueue_script(
        'myscript', // namnge ditt script.
        get_stylesheet_directory_uri() . '/js/myscript.js', // lagg till korrekt adress och namn pa skriptet
        array('jquery') // this array lists the scripts upon which your script depends
    );
}
add_action( 'wp_enqueue_scripts', 'add_my_script' );
```

Anledningen till att det är `get_stylesheet_directory_uri()` och inte `get_template_directory_uri()` är för att detta är ett child-tema. Om det inte hade varit då, då skulle du använt `get_template_directory_uri()`.
2. Lägg nu till namnet och korrekt adress till. I det här fallet heter skriptet myscript.js och ligger i mappen js, därför är det korrekt att skriva "/js/myscript.js".
3. Notera att i ditt script så kan du inte använda $, istället bör du använda ordet "jQuery". Här är ett exempel på ett skript och hur det bör se ut om det används i wordpress.

```javascript
jQuery(document).ready(function(){
    jQuery("img").click(function(){
    jQuery("img").fadeOut("fast");
    });
});
```
