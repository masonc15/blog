---
layout: post
title: Tekniker för att Centrera saker
date: 2015-09-08 17:41:54.000000000 -03:00
type: post
published: true
status: publish
categories:
- css
- flex
- html
- web-utveckling
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '14570267597'
  original_post_id: '470'
  _wp_old_slug: '470'
  _oembed_fd8c396ce5567825860af2334d89364f: "{{unknown}}"
  _oembed_cd464bb539ee0a42624076f6f9f09eca: "<div class=\"embed-css-tricks\"><blockquote
    class=\"wp-embedded-content\"><a href=\"https://css-tricks.com/almanac/properties/a/align-content/\">align-content</blockquote><script
    type='text/javascript'><!--//--><![CDATA[//><!--\t\t!function(a,b){\"use strict\";function
    c(){if(!e){e=!0;var a,c,d,f,g=-1!==navigator.appVersion.indexOf(\"MSIE 10\"),h=!!navigator.userAgent.match(/Trident.*rv:11./),i=b.querySelectorAll(\"iframe.wp-embedded-content\");for(c=0;c<i.length;c++)if(d=i[c],!d.getAttribute(\"data-secret\")){if(f=Math.random().toString(36).substr(2,10),d.src+=\"#?secret=\"+f,d.setAttribute(\"data-secret\",f),g||h)a=d.cloneNode(!0),a.removeAttribute(\"security\"),d.parentNode.replaceChild(a,d)}else;}}var
    d=!1,e=!1;if(b.querySelector)if(a.addEventListener)d=!0;if(a.wp=a.wp||{},!a.wp.receiveEmbedMessage)if(a.wp.receiveEmbedMessage=function(c){var
    d=c.data;if(d.secret||d.message||d.value)if(!/[^a-zA-Z0-9]/.test(d.secret)){var
    e,f,g,h,i,j=b.querySelectorAll('iframe[data-secret=\"'+d.secret+'\"]'),k=b.querySelectorAll('blockquote[data-secret=\"'+d.secret+'\"]');for(e=0;e<k.length;e++)k[e].style.display=\"none\";for(e=0;e<j.length;e++)if(f=j[e],c.source===f.contentWindow){if(f.removeAttribute(\"style\"),\"height\"===d.message){if(g=parseInt(d.value,10),g>1e3)g=1e3;else
    if(200>~~g)g=200;f.height=g}if(\"link\"===d.message)if(h=b.createElement(\"a\"),i=b.createElement(\"a\"),h.href=f.getAttribute(\"src\"),i.href=d.value,i.host===h.host)if(b.activeElement===f)a.top.location.href=d.value}else;}},d)a.addEventListener(\"message\",a.wp.receiveEmbedMessage,!1),b.addEventListener(\"DOMContentLoaded\",c,!1),a.addEventListener(\"load\",c,!1)}(window,document);//--><!]]></script><iframe
    sandbox=\"allow-scripts\" security=\"restricted\" src=\"https://css-tricks.com/almanac/properties/a/align-content/embed/\"
    width=\"500\" height=\"282\" title=\"&#8220;align-content&#8221; &#8212; CSS-Tricks\"
    frameborder=\"0\" marginwidth=\"0\" marginheight=\"0\" scrolling=\"no\" class=\"wp-embedded-content\"></iframe></div>"
  _oembed_time_cd464bb539ee0a42624076f6f9f09eca: '1468462315'
  _oembed_8358c551af935013a754bd5b8cb60619: "{{unknown}}"
  _oembed_db302cb19acfee9e281154495bd9fbfa: "{{unknown}}"
  _oembed_239f3f4de146813bb7c482f5004b6207: "{{unknown}}"
  _oembed_e4aae7dc24b512844fb1338585f586da: "{{unknown}}"
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Varje
https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties
https://css-tricks.com/almanac/properties/a/align-content/
https://philipwalton.github.io/solved-by-flexbox/demos/vertical-centering/
Centrera saker i flexbox. Du sätter display å justify å sådant. Som jag skrivit nedan i en flexboxcontainer. Då kommer Sakerna som finns i den diven att centreras.

```css
.flexboxcontainer{
width: 400px;
height: 400px;
display: flex;
flex-direction: row; /*Du kan välja mellan row och column */
justify-content: center;
align-items: center;
}
.box-inside-flexboxcontainer {
}
```

# Horizontell centrering

## Text-align
Text-align-tekniken är klassisk. Först skapar du en div-container. Denna container behöver ha en specificerad width för att det ska funka. Annars kommer den bara vara så står som innehållet i diven, å då går det ju inte att centrera.
Sedan behöver vi en div som ska centreras. Vi ger den en längd å bredd. Det viktigaste här är att komma ihåg att en div inte är en text, å en div är ett block-element, inte ett inline-element. Därför måste vi förvandla diven till ett inline-blocks-element. Det gör vi med display: inline-block. Då får diven inline-liknande egenskaper samtidigt som den är typ som ett block.

```css
.container {
width: 500px;
text-align: center;
}
.box {
width: 100px;
height: 100px;
display: inline-block;
}
```

## Margin-auto-tekniken

Det går helt enkelt ut på att du sätter marginalen på auto i innerdiven. Då justerar den sig automatiskt och blir därmed centrerad.

```css
.container {
width: 500px;
}
.box {
width: 100px;
height: 100px;
margin-left: auto;
margin-right: auto;
}

```

## Position-absolute-tekniken
Den här tekniken fungerar genom att sätta position absolute, å sedan sätta den på 50%. Detta innebär alltså att positionen är i mitten. Men eftersom diven i det här exemplet är 100px stor så innebär det att det är den vänstra sidan som är precis 50% in i container-diven. För att den ska bli helt centrerad måste vi därför justera marginalen med -50px (alltså hälften av diven storlek). Då blir den helt centrerad.

```css
.container {
width: 500px;
}
.box {
width: 100px;
height: 100px;
position: absolute;
left: 50%;
margin-left: -50px;
}
```

# Vertical centering

## Position-absolute-tekniken

Denna teknik är besläktad med tekniken ovan med samma namn. Men det finns några viktiga skillnader. För att innerdiven inte ska flyga iväg åt tjotahejti så måste vi göra .container till position: relative. Annars så flyger innerdiven upp å lägger sig med en margin på -50px från body. Å det vill vi inte.

```css
.container {
width: 500px;
position: relative;
}
.box {
width: 100px;
height: 100px;
position: absolute;
left: 50%;
margin-left: -50px;
top: 50%;
margin-top: -50px;
}
```
