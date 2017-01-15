---
layout: post
title: JavaScript och Object Oriented Programming
date: 2015-09-15 16:38:33.000000000 -03:00
type: post
published: true
status: publish
categories:
- javascript
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '14813379198'
  original_post_id: '478'
  _wp_old_slug: '478'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

## Vad är ett objekt?

Ett objekt i Javascript är som ett objekt i verkligheten. Ett objekt består av properties. Ett fysiskt objekt består också av properties. Till exempel: objektet kopp består av handtag : 1, design: true, vikt : 1 kg. Dessa properties beskriver objektet. Ett annat vanligt exempel är objektet bil. En bil har properties dörr : 4st, motor: 1, färg: grön. Å så vidare.
Ofta pratar man om property och property value.

```javascript
var myObject = {
name: "Lars", //Name är property och Lars är Property Value
age: 28
}
```

### Hur skapar man ett objekt i Javascript?

Det finns i huvudsak två sätt att skapa ett objekt i javascript: **object literal** och **object constructor**.
Object literal ser ut såhär:

```javascript
var myObject = {
name: "Lars",
age: 28
}
```

Constructor pattern ser ut såhär:

```javascript
var Employee = function(){
}
//eller såhär
function Employee(name, profession){
this.name = name;
this.profession = profession;
}
```

Employee är nu en Constructor-function som kan användas genom att använda keyword new. Så, för att skapa en ny emplyee. Gör du såhär:

```javascript
var philip = new Employee();
//philip är nu ett nytt objekt som vi skapade från Employee-funktionen.
```

### Hur lägger man till properties i ett objekt?

För att lägga till properties i ett objekt gör man såhär. Om man bygger vidare på exemplet som vi startat med ovan.

```javascript
philip.ålder = 23;
philip.namn = "Philip";
```

## Methods
Det finns private properties och public properties.
Objekt har sina egna funktioner, dessa kallas methods.
