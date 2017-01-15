---
layout: post
title: How to - MEAN-app
date: 2015-11-06 20:59:49.000000000 -03:00
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
  _publicize_job_id: '16607466502'
  original_post_id: '555'
  _wp_old_slug: '555'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

**Starta express-servern**

1. Skapa en mapp med app-namnet.
2. I mappen skriv `npm init`, eller något sådant för att automatiskt generera en package-file.
3. Skapa en `server.js`-fil, eller `app.js`.
 - Importera de moduler som du behöver: http, express, path, fs.
 - Routra de filer du vill servera.
 - Importera databasen
4. Öppna upp mongod, och dess shell mongo.
Skapa en databas med kommandot:

```
use namnPåDatabas
```

lägg till innehåll med kommandot:

```
db.usercollection.insert({"username" : "pelle88", "name" : "pelle"})
```

läs i databasen genom kommandot.

```
db.usercollection.find().pretty();
```

5. Skapa `index.jade`
- Lägg till text.
- För att koppla filen till databasen behöver man typ något slags each-statement eller något liknande.
6. Starta servern å beskåda verket.
