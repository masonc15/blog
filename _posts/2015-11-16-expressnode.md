---
layout: post
title: Express/node
date: 2015-11-16 14:37:08.000000000 -03:00
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
  _publicize_job_id: '16907778218'
  original_post_id: '569'
  _wp_old_slug: '569'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

# Hur man skapar en basic express-server och serverar en index-fil.
1. Skapa en mapp.
2. I terminalen i mappen skriva:
```
npm init
```
Detta kommando skapar en package.json fil. Detta fil innehåller metainformationen om din app. Den är nödvändig för att allt ska fungera.

3. Ladda ner express genom kommandot:
```
npm install express --save
```
Med tillägget `--save` så sparas modulen automatiskt i package-json-filen, så slipper du uppdatera den för hand.

4.Skapa en fil som heter `app.js` (den kan heta `server.js`, eller någonting annat. men praxis med express är att kalla den för `app.js`).
5. I filen skriv in följande kod:

```javascript
var express = require("express"); //Här importerar vi modulen express.
var path = require("path"); //Här importerar vi modulen path. Denna hjälper oss att enkelt hitta i vårt filsystem på datorn.
var app = express();
app.get("/", function(req, res){
res.sendFile(path.join(__dirname + "/index.html"));
});
app.listen(3000); //Säger åt servern att börja lyssna på port 3000 efter requests.
```

`app.get` - Här skickar vi en get-request, om någon söker efter sidan localhost:3000. Det är alltså själva index-sidan. Så om nån går in på index-sidan (alltså localhost:3000) så skickar browserna en request till servern. Servern besvarar då requesten med ett response (res). Responsen är att skicka en fil: `res.sendFile`. `path.join(__dirname + "/index.html")); - path.join(__dirname` tar fram hela adressen fram till index-filen. Så istället för att behöva skriva `/Users/userName/node-app/serve-static-file`. Så detta skulle alltså gå att skriva:
`res.sendFile(/Users/minDator/kod/serve-static-file/index.html"));`

6.Skapa index.html
Nu behöver vi bara skapa en index.html-fil för att skicka när servern har tagit emot en request. Så skapa en index.html-fil i samma mapp som app.js, med vanligt html-innehåll.
All kod finns att hämta [här](https://github.com/xapax/serve-static-file).

# Hur man skapar en basic express-server och serverar en dynamisk jade-fil
Skillnaden mellan detta exempel och exemplet ovan är väldigt liten. Den enda direkta skillnaden är att jade-filer ovandlas i servern till html. Så själva filen `index.jade` skickas inte, hur den omvandlas till `index.html` och skickas därefter.

- Skapade `package.json`
```
npm init
```

- ladda ner expess-modulen
```
npm install express --save
```
- ladda ner jade-modulen
```
npm install jade --save
```
- skapa en `app.js`-fil.

```javascript
var express = require("express"); //Här importerar vi modulen express.
var path = require("path"); //Här importerar vi modulen path. Denna hjälper oss att enkelt hitta i vårt filsystem på datorn.
var jade = require("jade"); //Här importerar vi modulen jade
var app = express();
app.get("/", function(req, res){
res.render("index", {title : "Testing jade"}));
});
app.listen(3000); //Säger åt servern att börja lyssna på port 3000 efter requests.
```

Det enda som egentligen skiljer sig här, jämfört med tidigre tutorial är att vi importerar modulen jade, genom `require("jade");`.
Sedan renderar vi filen istället för att skicka den. Eftersom vi inte vill skicka `index.jade` så måste vi göra om den till index.html, detta gör vi genom
`res.render("index")`
Sedan lägger vi till lite dynamiskt innehåll för att det är kul.
- Skapa mappen views och skapa däri filen `index.jade`.

```jade
doctype html
html
  head
    title= title
  body
    h1 This is part of the tutortial to render and serve a jade-file
```

Det enda som egentligen är konstigt här är `title= title`. Det är här vi infogar den dynamiska datan som vi lade till i app-js-filen.
`res.render` har som default-väg mappen views. Så om du lägger `index.jade` i root-mappen så kommer du få upp ett felmeddelande. Därför måste du skapa mappen views och lägga filen där i.

# Hur man skapar en basic express-app som är kopplad till en databas.
**Installera alla moduler**
```
npm init
npm install monk --save
npm install mongodb --save
npm install express --save
```
Monk är modulen som hjälper oss att hantera vår databas.
Mongodb är vår databas
Express är såklart modulen som hjälper oss med routering och http å sånt.
**Konfigurera databasen**
- Skapa mappen data
- Äppna terminalen och öppna mongodb genom kommandot
```
mongod --dbpath /Users/Dator/kod/node/database-app/data/
```
Eller till den korrekta adressen. När du skriver in det här kommandot som sparas databasen i mappen data.
- Gå till mappen data och starta mongo-konsollen med kommandot
```
mongo
```
- Skapa en databas med kommandot:
```
use db-app
```
Skapa sedan en collection och infoga något med kommandot:
```
db.info.insert({"namn" : "Lars"});
```
info är vad jag väljer att kalla min collection.
Skapa innehåll
- Skapa filen `app.js`

```javascript
//Importera nödvändiga moduler
var express = require("express");
var path = require("path");
var jade = require("jade");
var monk = require("monk");
var mongodb = require("mongodb");
//Spara databas i variabeln db. db-app är namnet på databasen jag skapade tidigare.
var db = monk("localhost:27017/db-app")
// Skapar en express-app
var app = express();
//Routrar till min stylesheetmapp och js-mapp.
app.use(express.static(__dirname + '/stylesheet'));
app.use(express.static(__dirname + '/js'));
//Routrar index.html till index-sidan.
app.get("/", function(req, res){
	res.sendFile(path.join(__dirname + "/index.html"));
});
//Detta gör vår databas tillgänglig för vår router.
app.use(function(req, res, next){
  req.db = db;
  next();
})
//Här sätter vi upp "sidan" db-app. Det är alltså sidan localhost:3000/db-app. Så om nån skickar en request till den sidan, så svarar vår server med att skicka vår databas.
app.get("/db-app", function(req, res){
	var db = req.db;
	var collection = db.get("info");
	collection.find({},{},function(e, docs){
		res.json(docs);
	});
});
app.listen(3000);
```
8. Skapa index.html, style.css och script.js.
index.html skapas i rootmappen.
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Database test</title>
	<link rel="stylesheet" href="/style.css">
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
	<script src="/script.js"></script>
</head>
<body>
	# Testing to connect to database
</body>
</html>
```

Ja, här är det inga konstigheter. Vi importerar bara jQuery och vår css och vårt javascript-fil.
- Gör ett ajax-get-call i script.js
Öppna script.js.

```javascript
$(document).ready(function(){
getData();
});
function getData(){
	$.getJSON("/db-app", function(data){
		console.log(data);
	});
};
```

Det här är inga konstigheter. Här skapar vi en funktion som heter getData och som körs automatiskt när sidan öppnas. I funktionen gör vi ett getJSON-call till sidan "/db-app". När vi tar emot datan så printar vi bara ut den i konsollen.
Sådär. Det var allt. Svårare än så är det inte. Det krävs alltså i princip bara 4 filer. `package.json`, `app.js`, `index.html`, och `script.js` för att skapa en server som är kopplad till en databas.
Koden finns att hämta här

# Hur skapar man en express-server som kan göra get och post-requests till en MongoDB-databas

```bash
npm init
# Installera moduler
npm install express --save
npm install mongodb --save
npm install body-parser --save
npm install monk --save
```

ett enklare sätt att skriva detta är:
```
npm install express mongodb body-parser monk --save
```
- Skapa en databas
Skapa mappen data
```
mongod --dbpath /path/till/data/mappen
```
Gå till mappen kör kommandot:
```
mongo
```
sedan:
```
use namnpådatabas
```
skapa en collection och sätt in data där
```
db.namnpåcollection.insert({"namn" : "Pelle", "age" : 34});
```
Kontrollera att datan finns i databasen:
``''
db.namnpåcollection.find().pretty();
```
Datanbasen är färdig.
- Skapa filen `app.js`
Här är filens innehåll. Det mesta bör vara bekant vid det här laget.

```javascript
//Importera moduler.
var express = require("express");
var path = require("path");
var bodyParser = require("body-parser");
var monk = require("monk");
var mongodb = require("mongodb");
//Koppla upp oss mot databasen
var db = monk("localhost:27017/namnpådatabas");
//Initierar expressappen.
var app = express();
//Detta är för att kunna serva våra statiska filer i css och js-mapparna.
app.use(express.static("css"));
app.use(express.static("js"));
//Här servar vi index.html-filen. Inga konstigheter.
app.get("/", function(req, res){
	res.sendFile(path.join(__dirname + "/index.html"));
})
//Här skickar vi databasens innehåll till sidan /databas. Denna sida plockas upp av ett ajax-call i filen script.js som vi kommer skapa senare. collection.find tar här en callbackfunktion, å docs i callbackfunktionen hänvisar till det som finns i databasen. Sedan skickar vi det som finns i databasen som response. Därav res.json(docs).
app.get("/databas", function(req, res){
	var collection = db.get("datacollection");
	collection.find({}, {}, function(e, docs){
		res.json(docs);
	})
})
//Detta är vad som kallas för middle-ware. Detta behövs för att servern ska kunna ta emot information från requesten som kommer härafter. Utan bodyParser så kan man inte få tag i datan.
app.use(bodyParser.urlencoded({
    extended: true
}));
app.use(bodyParser.json());
//Nu kommer vi vill routern som hanterar post. Här ser vi alltså att om det postas någonting till sidan /adduser så ska servern ta informationen req["body"] å spara den i variabeln namn. Sedan insertar vi datan i databasen. Det är viktigt här att sedan skicka ett svar till klienten. Svaret res.send("done"); annars så får man ett fel. Det är även fundamentalt att skicka någon information. Om ingen data skickas så får man också ett fel 500 Internal Server error.
app.post("/adduser", function(req, res){
    var name = req["body"];
    var collection = db.get("datacollection");
	collection.insert(name, function (err, doc) {
  if (err) throw err;
  console.log(doc);
  res.send("done");
	});
})
//Här säger vi åt servern att sätta igång å lyssna.
app.listen(3000);
```
- Skapa `index.html`
Aja, inga konstigheter här. Vi importerar jQuery. Sedan skapar vi ett enkelt formulär get vardera input ett eget id. Och sedan får knappen ett eget id.

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>MongoDB-app</title>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
	<script src="/script.js"></script>
	<link rel="stylesheet" href="/style.css">
</head>
<body>
	# Post your name and age to the database
	<form>
		<input id="name" type="text" placeholder="name">
		<input id="age" type="text" placeholder="age">
		<button id="btnAddUser">Submit</button>
	</form>
</body>
</html>
```

- Skapa filen `script.js`
Inga konstigheter här heller direkt.

```javascript
$(document).ready(function(){
//Först i vårt getJSON så tar vi emot data från sidan /databas, sedan loggar vi datan och printar sedan ut den på sidan.
$.getJSON("/databas", function(data){
	console.log(data);
	for (key in data){
		$("body").append(data[key].namn + "<br>");
	}
	})
//Sen har vi en knapp som kallar funktionen addUser när man klickar på den.
	$('#btnAddUser').on('click', addUser);
});
//Nu kommer vi nog till det viktigaste. Här har vi en variabel som fylls med värdena från vårt formulär. Sen har vi vårt ajax-call. Som skickar till url: /adduser. typen är POST. datan som den skickar är variabeln som vi deklarerar precis innan. Å typen är JSON.
function addUser(){
	var newUser = {
	    'namn': $('#name').val(),
	    'age': $('#age').val(),
	}
	console.log(newUser);
	$.ajax({
		  url: '/adduser',
	    type: 'POST',
	    data: newUser,
	    dataType: 'JSON'
	});
	 }
```
- Sådär. Det var allt. Nu är det bara att köra igång databasen och köra igång node med kommandot `node app.js`. Sen gå till localhost:/3000 Där kan du sen posta data till databasen.

## Hur man skapar en express-server och kopplar upp den med mongodb med hjälp av mongoose
```
npm install mongodb express mongoose --save
```
- Skapa app.js
- Starta mongod och mongo.

```javascript
var express = require("express");
var mongodb = require("mongodb");
var mongoose = require("mongoose");
var path = require("path");
//Starts express.
var app = express();
//Connects to the database. Databas is the name of the database.
//If it does not already exist mongo creates it.
mongoose.connect("mongodb://localhost/databas");
//Not sure what this does.
var db = mongoose.connection;
//This produces an error or success message when starting the server.
db.on("error", console.error.bind(console, 'connection error:'));
db.once("open", function(callback){
	console.log("database up and running")
});
//Here we create the database Schema. That is the format for how we are going to enter in data.
//We also create the name of the collection on the last line.
var FormSchema = new mongoose.Schema({
	name: String,
	created: Date,
}, {collection: "apa"});
//Here we create an instance of the database-schema that we just created.
var MyForm = mongoose.model("Form", FormSchema);
//Here we print out the documents found in the database.
MyForm.find(function(err, data){
	console.log(err);
	console.log(data);
})
//Here we are creating new data andsaving it in the database.
var form1 = new MyForm({"name" : "Form 1"});
form1.save();
//För att hitta med dess id gör man såhär:
MyForm.findById("56537c01e53134e80a1142c4", function(err, data){
	console.log(data);
});
```
