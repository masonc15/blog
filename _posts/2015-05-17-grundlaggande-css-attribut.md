---
layout: post
title: Grundläggande CSS-attribut
date: 2015-05-17 14:21:56.000000000 -03:00
type: post
published: true
status: publish
categories:
- css
- web-utveckling
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '13922412509'
  original_post_id: '261'
  _wp_old_slug: '261'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

## CSS - Cascading Stylesheet
Det finns en anledning till att det heter cascading. Det är för att det går uppifrån och ner. Dokumentet läses alltså uppifrån och ner.
Klasser överskriver taggar. Och ID överskriver klasser. Och Inline-styling överskriver ID.
Klasser överskrider varandra om de ligger längre ner i dokumentet. Det är alltså klasserna längre ner som överskriver övriga klasser. Men ID kan ligga var som helst i dokumentet och det överskriver ändå klasser.
Något som överskrider alla andra taggar, klasser, ID, Inline-style är !important.
Så kom ihåg. klasser längre ner i dokumentet överskrider klasser längre upp.

**Taggar. p, h1, header**
**Klasser - skriver över taggar**
**ID - skriver över klasser**
**Inline-style style="" - skriver över ID**
**!important - skriver över inline-style**
Lista och förklaring på de viktigaste CSS-attributen.


## Manipulera text
Det finns många sätt att manipulera text.

```css
text-transformation: uppercase; /*Gör om bokstäverna till stora bokstäver.*/
```

## Position
Här är en [](http://alistapart.com/article/css-positioning-101">fenomenal artikel som förklarar de fyra olika modellerna.
Attributet position har fyra olika alternativ.


```
position: static; //Default inställningen. Den mest grundläggande.
position: relative;
position: fixed; //Fixerad på viewporten.
position: absolute; //Absolut i förhållande till sidan.
```

**position: static; **

Den mest grundläggande positionen. Den kan bara lägga block på varandra. Ställa divvar ovanpå varandra.Du kan inte använda några top, bottom, left, right-attribut här inte.

**position: relative; **

Den fungerar lika bra som static med att bara stapla boxarna. Men, den har speciellt krafter. Det kan även kombineras med left, right, top, bottom.
Exempel:

```css
position: relative;
left: 200px;
```

I detta exempel så kommer boxen att röra sig 200px från vänster. Det betyder alltså 200px från vänsterkanten mot högerkanten. Om du skulle lägga till left: 200px; i en statisk så skulle ingenting hända.

**position: absolute; **

**position: fixed; **

**Boxmodellen**
[Boxmodellen](http://www.w3schools.com/css/css_boxmodel.asp) bör alla känna till. Varje DIV består av en margin, border, padding, content.
Margin är området utanför border.
Border är linjen som går runt objektet.
Padding är området mellan content och border.
Content är själva innehållet, t.ex. en text eller en bild.
Här är ett exempel

```css
div {
margin: 0px;
border: 5px solid black; /*5px tjock, solid och färgen svart */
padding: 20px 10px 100px 50px; /*EN för varje sida */
}
```

Om du lägger til fyran värden till attributed padding så definerar du varje sida.

**Box-sizing**

Ett väldigt bra attribut att känna till är box-sizing. Det kan vara väldigt förvirrande att arbeta med boxar. Även om man sätter 100% så innebär det inte alltid 100%, och 100px är oftast inte 100px, utan extra beroende på border, och padding. Men om man skapar box-sixing attributet så betyder height och width precis det som man skriver. Height är alltså 100px och varken mer eller mindre. Kom ihåg dock att margin inte räknas in! Men kom ihåg att attributet är relativt nytt och inte fullt implementerat, så glöm inte browserspecifika attributen.

```css
* {
-webkit-box-sizing: border-size;
-moz-box-sizing: border-size;
-ms-box-sizing: border-size;
box-sizing: border-size;
}
```

**Browserspecifika attribut**

Browserspecifika attribut är attribut som är specifika för vissa browsers. För attribut som inte är fullt implementerade, där det inte råder någon exakt definition av hur attributet ska implementeras i browser används dessa browserspecifika attribut. Det är ett sätt att säga: Så här vill FireFox implementera det, så här vill Chrome implementera det.

**-webkit** - Safari och Chrome

**-moz** - **Moz**illa Firefox

**-ms** - Internet Explorer

**-o** - Opera

För att veta om det är säkert att använda ett visst attribut kolla [Can I Use](http://caniuse.com). Ett exempel är attributet box-sizing: border-box;

**Definera alla element i css**

Något som kan vara effektivt ibland är att tillfoga alla element ett visst attribut. Till exempel om man håller på å bygger upp en struktur och vill att alla element ska ha en border-attribut då kan man göra det såhär.

```css
* {
    border: 1px solid red;
}
```

Stjärnan i början indikerar att alla element på sidan får det attributet. En annan användbar situation är följande.

```css
* {
    box-sizing: border-box;
}
```

Om du vill att alla boxar ska ha box-sixing attributet, som jag skrev om ovan, så kan stjärnan också vara användbar. Glöm inte de browserspecifika attributen.


**Media Quieres**

Ett av de viktigaste attributen för att göra en hemsida responsiv är med hjälp av media quieres. Det ser ut såhär:

**@media**

```css
@media only screen and (max-width: 300px) {
    p {
        background-color: blue;
    }
}
```

Här ser vi ett exempel på en media querie. ordet `only` hjälper oss med äldre webbläsare. ordet `screen` betyder att media querien gäller alla slags skärmar, paddor, telefoner eller laptops. Du kan även använda ordet `print` för att göra om hemsidan till utskriftsformat. `and` är nödvändig för att binda ihop kommandona.
(`max-width: 300px`) betyder att reglerna kommer endast gälla för skärmar som är mindre än 300px.

## Width: 100%

Om man sätter width 100%, och sedan sätter padding eller margin så kommer boxen att vara större än 100% vilket ju är störande. Så för att lösa det kan man lägga till css attributet:

```css
div {
box-sizing: content-box; /*Detta gör att margin och padding räknas in i 100%*/
box-sizing: border-box; /*Detta gör att margin och padding inte räknas in*/
}
```
