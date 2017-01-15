---
layout: post
title: FTP-kommandon
date: 2015-04-17 17:53:54.000000000 -03:00
type: post
published: true
status: publish
categories:
- how-to
- web-utveckling
tags:
- ftp
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '11134648407'
  original_post_id: '97'
  _wp_old_slug: '97'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Lista på FTP-kommandon att känna till. [Här](http://www.computerhope.com/issues/ch001246.htm) finns fler kommandon.

**cd**, **ls -la** , **pwd** och andra vanliga *nix-kommandon fungerar.

**get** - För att hämta filer från remote-dator. Ex. **get fil1**

**put** - För att skicka fil till remote-dator: **put fil1**

**mget** - För hämta flertal filer. Ex. **mget fil1 fil2 fil3**. För att snabbt ta hem filer är det smart att först inaktivera prompt-läget genom kommandot: **prompt**. Och sedan köra kommandot **mget ***. Då laddas alla filer i mappen ner automatiskt.

**mput** - För att ladda upp flertal filer. Ex. **mput fil1 fil2 fil3**. **mput *** fungerar här också. Glöm inte att inaktivera prompten innan.

**mkdir** - skapa en mapp

**rmdir** - ta bort mapp

**del** - för att ta bort fil ex. **del fil1**

**mdel** - ta bort flera filer. Ex. **mdel fil1 fil2 fil3**

**prompt** - för att inte bli för förfrågad för varje fil som du vill ta bort, ladda upp, ladda ner. Ex: **prompt** [enter] **mdel fil1 fil2 fil3**.

## Radera rekurisvt
För att ta bort alla filer i en mapp och alla filer som finns i den mappen så behöver man ladda ner lftp. Ftp verkar inte kunna göra det.
Så för att ladda ner lftp behöver du homebrew om du har mac. Om du har homebrew så kan du först köra:

```bash
brew doctor
brew update
brew install lftp
```

När lftp är nerladdat och installerat så loggar du in på följande sätt:

```bash
lftp
open -u user,password -p 21 adress
```

**Exempel**

```bash
lftp
open -u mittanvändarnamn,losenord -p 21 189.76.177.13
```
Sedan kan du använda kommandot: **rm -r mappnamn** för att rekursivt.
