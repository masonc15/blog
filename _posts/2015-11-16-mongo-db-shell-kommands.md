---
layout: post
title: Mongo-db shell kommands
date: 2015-11-16 17:49:21.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '16912812754'
  original_post_id: '573'
  _wp_old_slug: '573'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Fler kommandon finns [här](https://docs.mongodb.org/manual/reference/mongo-shell).

**Visa alla databaser**
```
show dbs
eller
show databases
```

**Visa vilken databas du är i nu**
```
db
```

**skapa en databas**
```
use namn-på-databas
```
**Infoga info i databas**
Först gå in i databasen genom use namn-på-databas sedan:
```
db.namn-på-collection.insert({"namn" : "Lars", "age" : 10});
```
**Byt till annan databas**
```
use namn-på-databas
```
**Via en databas olika collections**
```
show collections
```
**Visa de fem senaste kommandona**
```
show profile
```
**Visa innehållet i en viss collection.**
Först gå till rätt databas genom use sedan:
```
db.namn-på-collection.find()
För att göra det lite mer läsvändligt så lägg till pretty:
db.namn-på-collection.find().pretty();
```

**Ta bort en viss collection**
```
db.namn-på-collection.drop();
```

**Visa nuverande databas som jag är i**
```
db.getName();
```
**Ta bort en databas helt å hållet**
```
use namn-på-databas
db.dropDatabase();
```
**Ta bort en collection**
```
db.namnpåcollection.remove({});
```
**Uppdatera dokument**
```
db.namnpåcollection.update({firstName : "Lars"}, {$set : {"firstName" : "Jerry"}});
```
**Ta bort alla document i en collection**
```
db.namnpåcolletion.remove({});
```
