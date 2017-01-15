---
layout: post
title: Html-taggar
date: 2015-05-13 16:00:10.000000000 -03:00
type: post
published: true
status: publish
categories:
- web-utveckling
tags: []
meta:
  _publicize_pending: '1'
  geo_public: '0'
  _publicize_job_id: '10964770740'
  original_post_id: '210'
  _wp_old_slug: '210'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Det är viktigt att komma ihåg att taggarna ska användas på rätt sätt. Att använda section, article istället för en div hjälper sökmotorer, en person som enkelt försöker förstå koden, webbläsare och accessibility-tools att förstå hemsidan bättre. Respektera dom. Om du inte vet vilken tag som passar bäst så kan du följa den här [flow-charten](http://html5doctor.com/downloads/h5d-sectioning-flowchart.png). Här är en bra [artikel](http://html5doctor.com/lets-talk-about-semantics/) på detta tema.
Exempel, om du vill göra en några ord indenterade så ska du inte använda taggen **blockquote**, eftersom blockquote betyder att texten är ett citat. Om den är semantiskt dåligt skriven kan t.ex. synskadade användaren tvingas höra massa onödigt.
Man kan helt enkelt säga att semantic markup behövs för att hjälpa maskinerna att förstå nyanserna i människornas språk.
<img src="{{ site.baseurl }}/assets/h5d-sectioning-flowchart.png" alt="flowchart" />

## Taggar

**article**

Tänk inte på article som en tidningsartikel, utan som en sortimentartikel. En produkt, ett objekt.

**section**

**nav**

**figure**

**aside**

Inte samma sak som en sidebar. Utan snarare som en sido-del i en artikel. Någonting som relaterar till artiklen.

**footer**

**header**

### hr
HR står för horizontal rules och betyder helt enkelt horizontal linje. Det är en avgränsare som ofta används.

### IMG
Bilder ska befinna sig inom figure-tagen. Glöm inte figcaption om du vill ha caption-text.

```html
<figure>
<img src="bild.jpg"
alt="Bild på katt"
heigh="200px" width="200px"
title="namn på bilden">
<figcaption>Här är en text som kommer under bilden. </figcaption>
<figure>
```

Kom ihåg att alltid skriva ut alt eftersom det är viktigt för synskadade. Synskadades hjälpmedel läser upp vad som står i alt-attributet. För mer om hur du gör din sida screen-reader-friendly se [här](https://narcotize.wordpress.com/2015/05/24/screen-reader-friendly/).

### !DOCTYPE
Tagen är ingen html-tag. Det är information som browsern förstår vilket gör att den vet vilket typ av dokument som den ska läsa, och vilken version. Om html-dokumentet är skrivet i html5 så räcker taggen
```
html <!DOCTYPE html>
```
, om det är skrivet i html4, så kolla [här](http://www.w3schools.com/tags/tag_doctype.asp). För det mesta spelar det ingen roll, men om du vill vara säker på att din sida ser ut som den ska göra så lägg till !DOCTYPE.
### Tables
Tables är viktigt för att göra kolumner och rader.

### Forms

```html
<form>
<input type="text">
</input>
</form>
```

### Div

Div används på liknande sätt som span, alltså när ett mer adekvat element inte finns (div bör ej användas för section, header, footer,article). Div, liksom span, betyder ingenting i sig själv, utan används för att kunna implementera något annat. Tagen används ofta för att gruppera ihop ett antal olika element för att dom en specifik stil. Exempel:

```html
<div class="container">
# rubrik
paragraf med text
</div>
```

**Meta**

Metataggen används för att förmedla information som är tänkt för maskiner, och inte människor. De vanligaste är: name, charset.
Så här ska det se ut:

```html
<!-- Beskrivning av hemsidan. Används i vissa fall av google -->
<meta name="description" content="Här är en beskrivning av hemsidan" />
<!-- Används för att förmedla vilken character encoding hemsidan använder -->
<meta charset="utf-8">
<!-- Nyckelord. Används ej längre av google, men av Baidu. -->
<meta name="keywords" content="nyckelord, för, sökmotorer" />
```

För mer om hur google förhåller sig till metatags klicka [här](https://support.google.com/webmasters/answer/79812?hl=en). Om du sida riktar sig till kinesiska besökare så kan det fylla en funktion att använda keywords. Den största sökmotorn i kina, Baidu, använder sig nämligen av det. Mer om det [här](http://searchengineland.com/the-b2b-marketers-guide-to-baidu-seo-180658).

** Span**
Span används inom text. Om du till exempel vill ge ett specifikt ord i en mening en viss klass så kan du göra det genom att använda span. Exempel:

```html
 Här är en helt vanlig mening.
Där ett specifikt ord får en egen klass med hjälp av span.
Här <span class="unikt-ord">är</span> meningen.

```
Span bör dock endast användas om det inte finns en bättre tag att använda.

## Citat

```html
<blockquote>Detta är ett blockcitat</blockquote>
<q>Detta är ett litet citat</q>
<ins>Understrycker ett ord</ins>
<del>Drar ett streck genom ordet</del>
```
