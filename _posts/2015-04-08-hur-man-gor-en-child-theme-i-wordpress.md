---
layout: post
title: Hur man gör en child-theme i wordpress
date: 2015-04-08 00:45:55.000000000 -03:00
type: post
published: true
status: publish
categories:
- how-to
- teknik
- webbdesign
- wordpress
tags:
- child-tema
- wordpress
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  original_post_id: '57'
  _wp_old_slug: '57'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
I den här guiden ska vi se hur man gör ett child-tema i wordpress. I denna guide utgår vi ifrån att parent-temat heter Fashionistas.

1. Ladda ner och aktivera det tema som du vill ska utgöra parent-temat för ditt child-tema.

2. Skapa en mapp på servern där sidan ligger. Mappen skall ligga under /wp-content/themes. Namnge mappen något i stil med fashionistas-child, så att du minns vilket tema ditt child-tema är baserat på.

3. Kopiera parent-temats style.css-fil. Den ligger i wp-content/themes/namn-på-parent-tema/style.css.


- Om du har loggat in på din webbserver via ftp så kan du kopiera ner style.css med ftp-kommandot:
```
get style.css
```
Filen `style.css` kommer då att sparas i mappen du befinner dig i på den lokala datorn.

Öppna filen `style.css`. Det kommer då att se ut ungefär så här:

```css
/*Theme Name: Fashionistas

ChildTheme URI: http://athemes.com/theme/fashionista

Author: aThemes

Author URI: http://athemes.com

*/
```

4. Ändra namnet på temat. Lägg till texten Template: fashionistas.

5. Gå därefter till mappen `/wp-content/themes/parent-tema-namn-child` som du nyss skapade. Skriv in ftp-kommandot:

```
put style.css
```

6. Skapa filen `functions.php` och lägg in följande text:

```php
<?php
add_action( 'wp_enqueue_scripts', 'theme_enqueue_styles' );
function theme_enqueue_styles() {
    wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );

}
?>
```
Om du har fler än ett .css-fil så läs mer [här](https://codex.wordpress.org/Child_Themes).

7. Gå in i dashboarden till din hemsida och gå till teman, och klicka sedan på aktivera tema.
