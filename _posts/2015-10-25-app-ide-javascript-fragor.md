---
layout: post
title: 'App-idé: JavaScript-frågor'
date: 2015-10-25 15:23:57.000000000 -03:00
type: post
published: true
status: publish
categories:
- appidé
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '16191275527'
  original_post_id: '530'
  _wp_old_slug: '530'
  _oembed_85227e4979eea47be50a315dfe67c63d: "{{unknown}}"
  _oembed_1523c24299c882346a697da463895a63: "{{unknown}}"
  _oembed_8a7e04242b73c24e5f1830f0f951a861: "{{unknown}}"
  _oembed_7542f01863218a8ecfec3c1677a8585e: "{{unknown}}"
  _oembed_fc27afd9137a98d2d980ed198e53ae74: "{{unknown}}"
  _oembed_d6c97ea62da0c29bd3759af02fa66f36: "{{unknown}}"
  _oembed_c170f4d8cf24fff86471a0911ba14a0c: "{{unknown}}"
  _oembed_b9db38039f6ee46aad3e2ebee648519e: "{{unknown}}"
  _oembed_404b237e642c522da1a7b4b3bdcb54b3: "{{unknown}}"
  _oembed_066b985a1488c9238ac05212d9185af4: "{{unknown}}"
  _oembed_478fae52963afca1a45bbb689e20b71b: "{{unknown}}"
  _oembed_1cee6e4066205ad462bab7b7a37ffdbd: "{{unknown}}"
  _oembed_c795386866e3f0b14ff967be1e93b600: "{{unknown}}"
  _oembed_02332198ceed1e09b0d6873f15de3100: "{{unknown}}"
  _oembed_98638752002f82029b5494d19891558f: "{{unknown}}"
  _oembed_4f69f9d3dc49f6ca5d6690dc7737dbe6: "{{unknown}}"
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
## Prepare for JS-job

Göra ett frågespel med javascript-frågor. Med olika kategorier: typ fundamentals, challenges, olika koncept.
Sen kan användaren göra frågorna och bli bättre. Om en person inte kan frågan så markeras den med en viss poäng. Frågorna med lägsta poängen kommer upp först. Så att man hela tiden tränar på de frågorna som man är sämst på. Vid varje fråga finns en länk till en sida som förklarar konceptet mer utförligt.
Type såna här frågor: https://www.codementor.io/javascript/tutorial/top-ten-things-beginners-must-know-javascript
http://www.skilledup.com/articles/20-must-know-javascript-interview-qa
&nbsp;

## Frågor

### Closure

**What is lexical scoping?**

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures

**Is an Object mutable or imutable?**

Ett objekt är mutable. Detta innebär att objektets innehåll kan förändras. En primitiv data, så som string eller nummer kan inte ändras. Detta innebär att om du ändrar en variabels innehåll, så ändrar du det egentligen inte. Du lägger bara till det. Men det gamla värdet ligger fortfarande kvar i minnet. Detta gör att det kan vara mer krävnade. Det gamla värdet försvinner först när det är dags för garbage collection.
Ett object däremot, om du ändrar det så ändras värdet i minnet.

**Is a primitive data mutable or imutable?**

http://stackoverflow.com/questions/3200211/what-does-immutable-mean

**When is Javascript used outside of the browser?**

http://www.programmerinterview.com/index.php/javascript/javascript-outside-the-browser/

**What is ECMA-Script?**

ECMAScript is a standard for a scripting language, and the Javascript language is based on the ECMAScript standard.
http://www.programmerinterview.com/index.php/javascript/javascript-what-is-ecmascript/

**Which are the six primitive data-types in Javascript?**

Boolean
Null
Undefined
Number
String
Symbol (new in ECMAScript 6)

**Which data-type is Object data?**

Objects
Arrays and functions are also Objects.

**What is the difference between undefined and null?**

Undefined is a value that has been declared but not given a value.
Any variable that has not been given a specific value by default gets the value of undefined.
Null is a value. The value of nothing.

**What is the difference between `==` and `===`?**

`==` checks for equality of the value.

`===` checks for the equality of the value and type.

`"3" and 3 == true`

`"3" and 3 === false`

** Whats the difference between "var a = 1" and "a = 1"?**

`var` defines the variable locally, and it can not be used outside of that context.
With the `var` the variable is defined globally, and can thus be used outside of that scope. This might be bad practice as it can pollute the global scope.

**What does "1"+2+4 evaluate to?**

Since the first element is a string, the other value are also evaluated as strings.

**What does 1+2+"4" evaluate to?**

Evaluates to 34, because the first two numbers are evaluated arithmiacaly.

**What boolean operator do JavaScript support?**

Or: ||
And: &&
Not: !

**What would be the output of the following statements?

```javascript
var object1 = { same: 'same' };
var object2 = { same: 'same' };
console.log(object1 === object2);
```

False.
Because they are two different objects. They are not equal because they point at different objects.

**What would be the output of the following statements?

```javascript
var object1 = { same: 'same' };
var object2 = object1;
console.log(object1 === object2);
```

True. Because they reference the same object.

**What is this?**

```javascript
var arr = [[[]]];
```

A three dimensional array.

**What do you get if you do -10 / 0?**

-Inifinity
Known as negative infinity.

**What is the difference between an undeclared variable and an undefined variable?**

An undeclared variable is a variable that does not exist. An undeclared variable will result in an runtime error.

**What different type of loop do javascript support?**

```
For
for...in
while
do/while
```

**Mention the three ways to to convert a string to a number?**

```javascript
Number();
parseInt();
parseFloat();
```

**What does null mean?**

The NULL value is used to represent no value or no object.  It implies no object or null string, no valid boolean value, no number and no array object.

**What is an anonymous function?**

It is a function that has not been given a name, for example:

```javascript
var test = function(){
console.log("Hello world")}
//This, on the other hand, is not a anonymous function:
test(){
console.log("Hello world")}
```

**Is javascript case sensitive?**
Yes.
These are two different variables:

```
var test = 0;
var TEST = 1;
```

**Explain what hoisting is?**

Hoisting means that a function that is initialized will always be send to the top of the scope/function, but not with it's value, but just as a declared.
For example:

```
var test = 10;
function myFunction(){
console.log(test);
var test = 20;
}
//Consoles undefined
```

One would think that this function would console 10. But in fact it consoles undefined. Why is that then?
Because even though the variable is initliaized after the console.log-statement, is automatically becomes declared in the beginning of the function.
http://code.tutsplus.com/tutorials/javascript-hoisting-explained--net-15092

**What is the difference between declared variable and an initialized variable?**

A declared variable is a variable that has been declared with var. it could be:
```javascript
var a;
```

But an initialized variable is a variable that has been given a value.

**What is the difference between break, continue and return?**

`break` stops the loop and the continues after the loop.

`continues` stops the loop and then restart from the beginning.
`return` stops and leaves the function all together.


**Mention three different programming paradigms?**

**How do you create a default parameter in a function?**
This is a ECMA6-script. So at the moment it can only be used in firefox. But it works like this:

```javascript
function test(arg1, optionalArg = 23){
}
```

So if you don't put in any optional arg the default argument will be 23.


### Built in javascript functions

**What built in function would you use to find the index of a ascii symbol?**

```javascript
indexOf()
```

**What built in function would you use to get the index of a ascii symbol, starting from the end?**

```javascript
lastIndexOf();
```

**What function would you use to combine to lines of text?**

```javascript
text.concat("hej");
```

**What's the difference between `throw err` and `console.error("msg")`;**

Skillnaden är att `throw err` exit current clode block. Stops the execution. Medan `console.log` bara printar ut meddelandet i konsollen.


**What does IIFE stand for? And what is it for**

Det står för Immediately-invoked Function Expression.
Det är en funktion som är anonym och som executas direkt efter att den har skrivits. Den behöver alltså inte kallas för att den ska executa.
Här är en vanlig funktion. Den skulle inte executa eftersom den aldrig har blivit kallad.

```javascript
function testFunk(){
console.log("I am called now")};
```

Men om vi istället wrappar funktionen inom en IIFE, så kommer den automatiskt att kallas när den laddas.

```javascript
(function testFunk(){
console.log("I am called now")}());
```
För att undvika hoisting så kan det vara bra att använda IIFE.
Här är ett exempel på hoisting.

```javascript
var v = 1;
var getValue = function(x){
	console.log(x);
}
v = 2;
getValue(v);//Printar ut 2
```

Om vi här istället vill att funktionen ska printa ut det utsprungliga värdet så måste vi få funktionen att executa direkt när den skapas. Så här ser det ut:

```javascript
var v = 1;
var getValue = (function(x){
	return function(){console.log(x)}
}(v));
v = 2;
getValue();//Printar ut 1
```

Det är ett design pattern för att undvika variable hoisting.

**What six values are falsey in javascript?**

```
0 == false
undefined == false
null == false
NaN == false
"" == false << Alltså en tom string.
false == false
```
Detta kan vi enkelt testa genom att skriva:

```javascript
if (!0){console.log("False"};
```

Denna kod innebär alltså att om 0 inte är true så printa False.
Eller för att det inte ska vara lika förvirrande kan man skriva:

```javascript
if(NaN){console.log("I am true")}
else {console.log("I am false")};
```

Ett annat sätt att skriva samma sak är:

```javascript
!0 && console.log("False");
!NaN && console.log("False");
!undefined && console.log("False");
```

Allting annat är true.
Objekt, arrays, funktioner.
Nummer som är inte är noll.
All strings som inte är tomma.
True är true

För mer om detta läs [här](http://james.padolsey.com/javascript/truthy-falsey/)


**What is the difference between function-expression and function declaration?**


Function expression = `var functionName = function(){};`
Function declaration = `function functionName(){};`

**Vad betyder varje siffra i versionen 4.3.0?**


Major.Minor.Patch

`^4.3.0` - Version mellan 4 och 5. För att det inte ska bryta med versionen.


**What is a ternary operations and how does it work?**

Ternary operations are shorthand way to write an if/else statement.

```javascript
if(hello){
console.log("It is true")
}
else {
console.log("it is false");
}
//This can be written as following:
console.log(hello ? "It is true" : "It is false");
```

So question-mark means "if", and : means "else";

**Does javascript have blockscope or function scope and what does that mean?**

It has function-scope but not block scope. One would think that it has block-scope since it has that type of syntax, but in fact it only has function-scope.
This means that a variable that is defined within a

**Which are the ways you can create an Object?**

```javascript
//Object literal
var myObj = {name: "My Object"};
//Constructor
var myObj2 = new Object();
myObj2.name = "My Object 2";
```

**What is a potential pitfall with using typeof bar === "object" to determine if bar is an object? How can this pitfall be avoided?**

Problemet är att null också betraktas som ett objekt. Desamma gäller för arrays också.

**What will the code below output to the console and why?**

```javascript(function(){
  var a = b = 3;
})();
console.log("a defined? " + (typeof a !== 'undefined'));
console.log("b defined? " + (typeof b !== 'undefined'));
```

Här tror man lätt att `var a = b =3`; är likvärdigt med:
```javascript
var a = b;
var b = 3;
```

Men det gör det inte. Egentligen står det:
```
var a = b;
b = 3;
```
Vilket gör `b` till en global variabel.

**What is the significance, and what are the benefits, of including 'use strict' at the beginning of a JavaScript source file?**
`use stric` introducerades i ECMA5.

1. Det tvingar scripet till striktare parsing och error handling. Dessa errors skulle annars kanske ignoreras, eller eller failed silently. Nu genererar de istället ett error. So in det är good practice.
2. Det gör det därmed enklare att debugga. Eftersom man får upp fler errors.
3. Ett enkelt exempel är om man gör en variabel global när den redan är global.

```javascript
minVar = "test";
```

Detta generar ett fel när man kör i "use strict" men inte annars.

**What is closure?**

Closure är en inre funktion som har tillgång till de yttre funktionernas variables.

**What is Const?**

en constant variables

**What is a lambda function?**

It is a anonymous function that is saved in a variable.

Example:

```javascript
var lambda = function(saludos)
{
console.log(saludos);
}
lambda("hola");
```

## Node

**What is the difference between console.log/console.error and throw err?**
The main difference is that throw err breaks the event loop and leaves the function. Console.log(err)/console.error(err) doesn't break the function. It used prints our the error and then moves on.

**What is the event handler in node?**

**What's the difference between token-based authentication and cookie-based authentication?**

För att en användare inte ska behöva verifiera sig själv, eller alltså att logga in varje gång en ny sida besökt så använder man cookies eller tokens.
Så när en användare för första gången loggar in på en sida så svarar servern med att skicka en cookie, som innehåller ett sessions-ID. Det kan vara en lång string med nummer och bokstäver. För varje gång som användaren sedan går till en ny sida så skickas cookien tillbaka till servern och den verifieras. På så sätt behöver inte användare skriva in sina uppgifter varje gång hen besöker en ny sida. Det blir både säkrare mer användarvänligt.
Så, det finns alltså två huvudsakliga sätt att verifiera en användare. Det ena är med coookies och den andra med tokens. De har både sina för och nackdelar.
**Cookies**
**Tokens**
https://auth0.com/blog/2014/01/07/angularjs-authentication-with-cookies-vs-token/

## Algoritmer

**Kan du skriva ett program som tar reda på om ett tal är ett prim-tal eller inte?**

```javascript
//Check to se if a number is a prime number
function isPrime(num){
	var divisor = 2;
	while (num > divisor){
		if (num % divisor == 0){
			return console.log(false);
		}
		else {
			divisor++;
		}
	}
	return console.log(true);
}
isPrime(15);
```

**Kan du skriva ett program som skriver ut fibonacci-sekvensen med x antal?**

```javascript
//How to get the nth fibonacci-number
function fibonacci(n){
  var fibo = [0, 1];
  if (n <= 2) return 1;
  for (var i = 2; i <=n; i++ ){
   fibo[i] = fibo[i-1]+fibo[i-2];
   //fibo.push[i];
  }
 return console.log(fibo);
}
fibonacci(12);
```

## Data structures

**Vad är en data-struktur? Vad används de till och ge exempel på några vanliga datastrukturer i javascript?**

Här är en fantastiskt serie om datastrukturer.
http://code.tutsplus.com/tutorials/data-structures-with-javascript-whats-a-data-structure--cms-23347
Så en datastruktur är helt enkelt en viss slags struktur av data. Data kan struktureras på många olika sätt. Ett exempel som tas upp i tutorialen som jag länkar till ovan använder sig av exemplet med böcker. Vara bok är en data, och tillsammans är de data (i pluralis). Denna data, alltså dessa böcker, kan organiseras och struktureras på olika sätt. Ett sätt är att ställa alla böckerna i bokstavsordning i en bokhylla. Böckerna kan därefter struktureras efter författarens för ellere efternamn eller efter titel. Fördelen med detta sätt är att vi kan ta ut en bok hur som helst, från vilket position som helst i strukturen.
Om vi däremot ställer upp böckerna på en hög (stack). Så kan vi inte bara dra ut den bok vi vill ha, för då faller ju hela högen. Så om vi vill ta fram en av böckerna som är längst ner i stacken så måste vi ta bort alla böcker ovan, en i taget.****
