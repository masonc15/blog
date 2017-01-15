---
layout: post
title: Grundläggande JavaScript
date: 2015-06-07 20:56:08.000000000 -03:00
type: post
published: true
status: publish
categories:
- javascript
- web-utveckling
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '11438256740'
  original_post_id: '356'
  _wp_old_slug: '356'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

### Inbyggda funktioner/methods
**Manipulera stringar**
`.split()` - Delar upp en sträng och lägger den i en array.
[.splice()](http://www.w3schools.com/jsref/jsref_splice.asp) - Lägg till eller ta bort element, eller byt ut element i en array.
`.reverse();`

`.substr();` //Tar två parametrar. Den första är var beskäringen ska börja. Den andra parametern specificerar hur många karaktärer som ska beskäras. Den andra parametern är valfri.

`.substring();` //Detta är den äldre versionen av substr(); och gör samma sak. Skillnaden är den andra parametern. Substrings andra parameter är positionen efter den sista karaktären som du vill åt. Den andra parametern är valfri.

`.sort();`

`.replace();`

`.indexOf();` //Används för att söka igenom en string.

`.lastIndexOf();` //Samma som indexOf fast den börjar söka bakifrån.

`.charAt();` //Har en parameter, som tar indexnumret av av en viss karaktär och returerar karaktären.

```javascript
var string = "Har är min string";
string.charAt(0);//returerar H
```

`charCodeAt();` //Samma som charAt, med skillnaden att den returerar ASCII numret på karaktären.

[trim();](http://www.w3schools.com/jsref/jsref_trim_string.asp) //Tar bort överflödigt whitespace i början och slutet av en string.

**Manipulera nummer**

`.toFixed(1);` - Antalet decimaler ett tal ska ha.

`.parseInt();` - Denna funktion gör om stringar till tal. Det är viktigt att komma ihåg att den tar två argument, varav det andra argumentet definerar vilket bas talet ska vara
i. För att omvandla talet till base-10 så ska man alltså lägga till 10. Exempel:

```javascript
parseInt("5", 10);
```

[typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof) //För att se vad för typ av element något är. Om det är boolean, string etc.
[filter();](https://msdn.microsoft.com/library/ff679973(v=vs.94).aspx) Filtrerar bort element från en array.

**Iterative Methods - loopa utan att använda loop**
Dessa methods används för att loopa igenom arrays, utan att använda en loop. Dessa kallas därför för Iterative Methods. Det finns fem Iterative Methods.

`every();` Går igenom en array och avgör om ett visst condition är sant eller falskt. Till exempel.

```javascript
var numbers = [1,2,3,4,5];
function isLessThan3(value, index, array){
var returnValue = false;
if (value < 3){
returnValue = true;
 }
return returnValue;
}
var trueOrFalse = numbers.every(isLessThan3);
console.log(trueOrFalse); //Returns false.
```

Eftersom the condition som finns i if-statementet är falskt. value är inte mindre än tre, eftersom värderna 4 och 5 finns där. Och de är inte mindre än tre. every() är alltså en method som avgör om varje del i en array är sann i förhållande till condition-statementet. Om du däremot skapar samma funktion, men använder det för some(); så kommer det att returera true, eftersom några av values är mindre än tre, vilket gör påståendet till sant.
`some();`
`every()` och `some()` är nyttiga, men du kan inte få ut någonting mer från dom än true och false. Det är därför som filter är så bra.

**filter(); **
Filter gör precis som `every()` och `some()` och iterate genom en array. Men varje gång som påståendet är sant så läggs det värdet till i en ny array.
`map();` är en väldigt användbar funktion. Den används för att söka igenom en array och göra någonting.

```javascript
var array = [1,2,3,4,5];
//Om jag nu vill lägga till 1 på varje element inom arrayen så kan jag göra det med map, såhär:
var array = array.map(function(val){
  return val+1;
});
console.log(array); //[2,3,4,5,6]
```

`reduce()` - lägger ihop samtliga tal i en array till ett tal.

```javascript
var array = [4,5,6,7,8];
var singleVal = 0;
var singleVal = array.reduce(function(previousVal, currentVal){
  return previousVal+currentVal; //returns 30
});
```

`filter()` - filter filtrerar bort alla delar i en array. Vad som ska filtereras bort defineras i funktionen. Exempel:

```javascript
var array = [1,2,3,4,5,6,7,8,9,10];
array = array.filter(function(val){
  return val <= 5; //filtrerar bort alla tal som över över 5
});
```

`concat();` används för att slå ihop två arrayer.

```javascript
var array = [1,2,3];
var concatMe = [4,5,6];
array = array.concat(concatMe);
```

**Matematik-methods**

Det finns många methods rörande matematik.

```javascript
Math.PI; Get PI
Math.Ceil(2.45); Rundar upp
Math.floor(2.45); Rundar ner
Math.round(2.45); Rundar av mot närmsta heltal
```

### Object

Object i Javascript är som en dictionary i Python. Ordet Dictionary beskriver bättre vad det handlar om. Ett object består av en key och en value.

```javascript
var person = {
namn: "Philip",
age: "34"
}
```

### Listor/arrays

Såhär skapar man en lista.

```javascript
//Skapa en lista
var lista = ["namn", "djur", "nummer", 9, 8, 10000]

//Sortera en lista
lista.sort();
//Detta behövs för att listan skall sortera nummer som nummer och inte som strängar.
list.sort(function(a,b) {
return (+a) - (+b);
});
//Vänd listan bakochfram
lista.reverse()
//Ta ut delar av en lista och gör om den till en egen lista.
sliced = list.slice(0, 2) //Den första parametern är var slicen börjar, och den andra parametern (2) är hur många objekt ur lista du vill ta ut.
//För att ta reda på om en ett visst objekt finns i en array, gör såhär.
list.indexOf("namn"); //Svaret blir positionen i listan, i detta fall 0
//Push och pop.
//För att lägga till i en array:
myArray.push(["detta laggs till langs bak i listan"]);
//För att ta bort det sista elementet i en array
myArray.pop();
//Shift och unshift
//För att ta bort det första elementet i en array
myArray.shift();
//För att lägga till det första elementet i en array
myArray.unshift();
```

### Kopiera

Gör följande för att kopiera en lista.
```javascript
var lista = ["Hej", "apa", 6, 7];
kopiaAvLista = lista.concat();
```

Nu är kopiaAvLista en kopia av listan. Helt separat.
Om du däremot gör såhär:
```javascript
var lista = ["Hej", "apa", 6, 7];
länkTillLista = lista
```

Då skapar du i princip bara en länk till listan.

**console.log**
Används för att printa ut något, det motsvarar pythons **print**
```javascript
console.log("Vad som ska skrivas");
```

**var**
var används för att skapa en variabel.

```javascript
var = firstName = "James";
var age = 20;
console.log(firstname, age);
```

**if**

```javascript
if ( "johan".length > 10 )
{
    console.log("Let's go down the first road!");
}
else
{
    console.log("Du har ett kort namn");// What should we do if the condition is false? Fill in here:
}
```
