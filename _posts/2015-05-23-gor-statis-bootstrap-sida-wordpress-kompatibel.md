---
layout: post
title: Gör statis bootstrap-sida wordpress-kompatibel
date: 2015-05-23 01:02:39.000000000 -03:00
type: post
published: true
status: publish
categories:
- bootstrap
- css
- web-utveckling
- wordpress
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '10908282602'
  original_post_id: '314'
  _wp_old_slug: '314'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Det krävs en del knep å knåpande för att få en statisk html-sida att fungera i Wordpress.

## 1. Skapa tema-mapp
Skapa en mapp i wp-content/themes/Saft. Jag har valt att kalla vårt låtsastema för Saft i den här övningen.

## 2. Skapa style.css
I mappen saft måste du lägga till filen style.css. Det smidigaste är att du tar din .css-fil som du har använt och helt enkelt byter namn på den till style.css. Därefter kopierar du in följande kod längst upp i style.css. Fast men din egen info såklart.
```css
/*
Theme Name: Saft
Theme URI: http://www.saft.com
Author: Ditt namn
Author URI: https://www.saft.com
Description: Single page website.
Version: 1.0
License: GNU General Public License v2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html
Tags: images, scroll
This theme is licensed under the GPL.
Use it to make something cool, have fun, and share what you've learned with others.
*/
```

De flesta av de här uppgifterna tror jag är frivilliga. Det enda som är absolut nödvändigt är att ha Theme name.

## 3. Dela upp index.html-filen.
Wordpress är konstruerat så att header och footer är desamma på alla sidor. Om du klickar på en bloggpost så kommer header och footer antagligen se likadana ut som på första sidan. Det är för att index-filen är uppdelad.

 Eftersom vi kommer använda oss av programmeringsspråket php så behöver du ändra namn på dina filer. byt namn på `index.html` till `index.php`.
 Klipper du ut alla rader i index.php som ingår i headern. Vanligtvis från html-taggen ner till efter menyn och din header. Klipp ut det och klistra in det i filen `header.php`. Filen måste heta exakt så för att wordpress ska förstå den.
 Gör samma sak med footer-koden. Lägg all kod som hör till footern i `footer.php`
Om du har en sidebar så gör samma sak med den koden. Klipp ut den och lägg den i `sidebar.php`

## 4. Lägg upp det på din server.

Om du behöver hjälp med ftp-kommandon se här. Filerna ska ligga i mappen `/wp-content/themes/Saft`.

## 5. Aktivera temat
Sådär, nu kan du logga in på sidan via wordpress-loginen som vanligtvis är `saft.com/wp-admin`.

## 6. Knyt ihop.
Så, om du nu går in på din hemsida så ser du att din header och footer inte visas. För att visa den måste du redigera index.php-filen.
Precis i början av filen lägger du därför till.

```php
<?php get_header(); ?>
```

I slutet av `index.php` lägger du till

```php
<?php get_sidebar(); ?>
```

```php
<?php get_footer(); ?>
```
Man kan säga att dessa små koder är som länkar som binder samman filerna. Om du sprarar index.php å går till din hemsida kan du nu se header och footer.

## 7. Css
Sådär, nu är sidans innehåll på plats. Men sidan har ingen CSS. För att koppla in css måste du gå in i `header.php` och lägga till följande kod.

```html
<link rel="stylesheet" type="text/css" media="all" href="<?php bloginfo( 'stylesheet_url' ); ?>" />
```

Såhär, om du nu går in på hemsidan så ska sidan ha fått sin css. I alla fall style.css. Men för att koppla ihop bootstrap-css måste du lägga till följande kod.

```html
<link rel="stylesheet" href="<?php bloginfo('stylesheet_directory'); ?>/css/bootstrap.min.css" type="text/css" media="screen" />
```

## Bilder
Dina bilder har nu en ny plats på servern. Därför måste du ändra bildadresserna i din html-kod.
Om du har lagt till bilderna direkt i html-koden måste du ändra till följande adress:
`wp-content/themes/saft/img/bild.jpg` eller den korrekta adressen. Men bildens adress skall börja från wp-content.
Exempel:

```html
<img src="wp-content/themes/butikboot/img/web.png" class"img-responsive" alt="Cinque Terre">
```

Om du däremot har lagt till bilderna från ditt stylesheet så blir adressen relativ i förhållande till ditt `style.css`-dokument. Därför ska adressen börja vara img/bild.jpg.
Det skall exempelvis se ut såhär:

```css
background:url(img/28H.jpg) no-repeat center center fixed;
```

Om du skriver in adressen korrekt men det ändå inte händer något. Bilderna inte laddas, så kan det vara ett problem med att gamla css har cachats på servern. För att lösa det problemet, kolla min bloggpost om det [här](https://narcotize.wordpress.com/2015/04/14/langsam-uppdatering-av-css).
