---
layout: post
title: Bra bashkommandon
date: 2015-04-24 15:44:53.000000000 -03:00
type: post
published: true
status: publish
categories:
- how-to
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '16328665013'
  original_post_id: '136'
  _wp_old_slug: '136'
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
**Allmänt**
win; stoppar tidigare commando

# - betyder kommentar

**Söka**

Söka igenom filer efter specifikt ord. `-R` betyder att den söker rekursivt, alltså går igenom undermappar.
```
grep -R [ord] /Users/davocomp/mapp
```

**Exempel:**
```
grep -R hejsan /Users/davocomp/mapp
```
Ett annat sätt att söka är att använda find.
```
find ~/dinMapp -iname "*ord*"
```

Detta gör att du söker i mappen dinMapp, -iname gör att du söker stora och små bokstäver. `*` är en wildcard innan och efter sökningsordet.

**Öppna application**

på mac är det:
```
open -a namnpåapplication
```

**Stänga applicationer**

**osascript -e 'quit app "APPLICATIONNAME"'**

Exempel:
```
osascript -e 'quit app "Calendar"'
```

Ett annat är att hitta applicationens PID å sen stänga genom det.
det kan göras genom kommandot
```bash
pgrep Safari
ps -ax | grep Safari
kill 467
# eller tillsammans:
kill `pgrep 456`
```

ett annat sätt är att köra följande kombination


`pgrep Skype | xargs kill`

**TOP**

För att visa vilka processorer som är aktiva så kan man använda top.
För att se vilka processer som kostar mest så använd:
```
top -o cpu
```

**Kopiera**

```bash
cp namnpafil namnpakopieradfil
cp /plats/fran/var/filen/Finns.tx /till/plats/du/vill/kopiera
#Ta bort
rm namnpåfil
rmdir namnpåmapp
```

**Lista filer**

**ls**

**la**

**ls -S** För att sortera efter size.

**Ladda ner**

curl är ett bra sätt att ladda ner sidor

curl http://www.dn.se

om du sedan vill spara hela html-dokumentet i en fil så gör du

curl http://www.dn.se >> namnpåfil
om du vill söka igenom filen och bara spara de rader som berör något specifik så kan du använda:

curl http://www.dn.se | egrep sökning >> output
För att ta bort alla filer och mappar som finns i en mapp använd följande kommando med stor försiktighet. Det krävs sudo för att köra det. Och om du använder det fel, å tar bort hela filsystemet så är datorn körd.

```
rm -rf namnpåmapp
```

**Skapa ett helt directory tree med ett kommando**
Extremt användbart.
`mkdir -p public/{js,css} data views/{controller,views}`
Detta kommando kommer alltså skapa ett träd som ser ut som följande
```
public|
---------js|
---------css|
data|
view|
-------controller
-------views
```
