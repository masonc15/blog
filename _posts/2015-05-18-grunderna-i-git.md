---
layout: post
title: Grunderna i Git
date: 2015-05-18 22:30:27.000000000 -03:00
type: post
published: true
status: publish
categories:
- git
- web-utveckling
tags: []
meta:
  _publicize_pending: '1'
  sharing_disabled: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '13628202139'
  original_post_id: '268'
  _wp_old_slug: '268'
  _oembed_145e5c286688b309ab051a3c4dc22e09: "{{unknown}}"
  _oembed_585a6c50f6753b2342ab4b40d3a3bd36: "{{unknown}}"
author:
  login: tidlost
  email: philip.linghammar@protonmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
## Grundläggande Git-kunskap
Git är väldigt enkelt när man väl förstått grunderna i det. Låt oss säga att du har skrivit ihop en hemsida. Men du har börjat tröttna på att tappa kontrollen över alla ändringarna du har gjort. För att få kontroll över alla ändringar du gör i dina dokument kan lägga in dokumenten i en repository (repo). Så, låt oss säga att du har en mapp som heter hemsida, och i den mappen har du flera undermappar och dokument.
För att kunna lägga in alla dokumenten i en repository måste du först skapa en repository. Det gör du genom att gå in i mappen ~/hemsida och därefter skriva in kommandot

**git init**

Därmed har du skapat ett repository. Du har samtidigt skapat en mapp som hetet .git som innehåller olika konfigurationsfiler. Om du vill ta bort reposet så är det bara att ta bort .git-mappen (`rm -rf .git`).
Så nu har du skapat ett repo, men än så länge finns inga filer i reposet. Det kan du se om du skriver in kommandot:

**git status**

Då får du en lista på untracked files. ALltså filer som finns i mappen men som inte versionshanteras av git. Eftersom det är precis vad du vill att git ska göra så kan du lägga till filerna. Det gör du med kommandot:

**git add filnamn**

Om du har många filer och vill lägga till alla filer och mappar så använd kommandot:

**git add .**

Om du nu skriver git status så ser du en lista på alla filer som har lagts till. Filerna ligger i reposet men nu måste du commita dom. Att commita betyder helt enkelt att du säger "Nu har jag gjort en ändring i den här filen". Commita gör du genom att skriva kommandot:

**git commit -m "Kommentar på vad det är du har ändrat"**
Om du utelämnar `-m` så hamnar du i ett vim-dokument där du kan skriva längre och mer avancerade kommentarer.
Så, låt oss säga att du har lagt till lite kod i index.html, för att lägga till det i reposet måste du först lägga till filen, alltså:

**git add index.html**
Därefter kan du commita. Glöm inte att skriva ut tydliga och välformulerade commit-kommentarer, så att du själv minns varför du gjort en visst commit.
Låt oss säga att du fuckar upp det i index.html och ångrar några ändringar du har gjort. Då vill du se dina senaste ändringar. Använd då kommandot:

**git log**

Om du vill få en GUI-översikt så använd istället GitHub for Mac eller något annat GUI som passar dig.
En väldigt bra funktion i Git är förgreningen. För att starta en ny gren använder du kommandot:

**git branch namnpånygren**

för att sedan gå över till den grejen använd kommandot:

**git checkout namnpågren**

Om du sedan ändrar och commitar i grenen så finns den ändringen bara i grenene. Om du sedan går tillbaka till masterversionen genom kommandot

**git checkout master**

så kommer du att inga förändringar har skett i masterdokumenter.
Om du sen gör förändringar i den nya branchen och sedan vill merge brancherna när du är klar. Gå till master-branchen och kör:

**git merge namnPåBranch**

Sedan addar och commitar och pushar du.
Skapa en ny remote branch och pusha till den:
skapa en ny branch och commita till den:

**git checkout -b test**

Pusha till en ny remote branch

**git push origin test**

Ta bort lokal och remote branch.

**git branch -d the_local_branch**

**git push origin - -delete the_remote_branch**

## Lista på kommandon

För att skapa ett nytt gitprojekt (repository - repo) skriver du bara

**git init namnpåprojekt**

Eller så skapar du en mapp och går in i den och skriver sedan: **git init**
För att se status över reposet

**git status**

Lägga till filer i reposet

**git add filnamn**

Lägga till alla filer i reposet

**git add .**

Ta bort en fil från reposet

**git rm --cached index.html**.

Filen tas då inte bort från datorn utan bara från reposet, git kommer alltså inte att följa filens förändringar.

**.gitignore**

Du kan skapa en fil som heter .gitignore och i filen sedan lista alla filer och mappar som du inte vill att git ska tracka. Exempel på dessa är till exempel:
data - om du har en databas
node_modules
Om du råkar tracka dom, och sedan ångrar dig och vill ta bort dom från reposet så kör:

**git rm [-r om det är en mapp] --cached data**

Att ladda ner/kopiera ett visst projekt från github. Använd kommandot clone. Exempel:

**git clone https://github.com/användarnamn/projektnamn**

Visa logs över tidigare commits och dess ändringar:

**git log**

Detta visar tidigare commits

**git log -p**

Detta visar tidigare commits och exakt vilka ändringar som gjorts

**git log -p 2**

Visar commits och ändringar i de senaste två commitsen
En genväg för att skapa en branch å gå till den.
För att göra detta:
```
git branch iss53
git checkout iss53
Kan man göra:
git checkout -b iss53
```
Få tillgång till remote git-repos.
Dom är av någon konstig anledning typ dolda. Så man måste köra:

**git branch -a**

eller

**git branch --all**

För att kunna se dom.
Du kan titta på de osynliga brancherna genom:

**git checkout origin/orig.org**

Men för att arbeta på någon av dom så måste du skapa en lokal branch av den remota, det gör man genom följande kommando:

**git checkout -b orig.org origin/orig.org**

## Lägga upp till remote git-repo
Det finns två sätt att connecta med ett remote repo.

**Sätt nr 1**
1. Det enklaste sättet är att först gå in på github.com å skapa ett repo. Kom ihåg att lägga till en readme.md-fil. Det är nödvändigt för att du ska kunna klona reposet.
2. Gå in i terminalen och skriv **git clone https://github.com/xapax/Voting.git** eller vad nu adressen till reposet är.
3. Nu har du klonat reposet och det finns på din dator. Om du nu gör några ändringar som du sedan addar och commitar så måste du skicka dom ändringarna till till remote-repo. Det gör du genom följande kommando:
`git push origin master`

Kommandot push tar två parametrar, den första, origin, är remote reposet och det andra, master, är den lokala grenen som du vill pusha.
**Sätt nr 2**
```
mdkir mitt-repo
cd mitt-repo
git init
vim readme.md** - Skriv in något i filen
git add .
git commit -m "Meddelande"
```
Skapa ett repo på github.com med exakt samma namn. Lägg inte till nån readme eller annan fil.

```
git remote add origin https://github.com/xapax/namn-på-dit-git-repo.git
```

```
git push -u origin master
```

## Lägga upp på github.com och göra en egen sida för koden
1. Gå in på github.com och skapa ett nytt repo.
2. Gå in i den mapp du har dina filer i. Kontrollera noga så att det inte redan finns ett repo i mappen. Kontrollera att det inte finns någon .git-fil. Det kan du göra med `ls -la`.
Därefter startar du ett repo på din dator. I mappen med filerna skriver du
```bash
git init
git checkout --orphan gh-pages # Skapar en ny gren och byter samtidigt över till den.
git add . # Då lägger du till alla filerna.
git commit -m "First commit" # Så committar du alla filerna.
git push origin gh-pages # Nu har du pushat upp alla filerna och appen bör finnas på adressen https://username.github.io/namnpårepo
```

## Hålla gh-pages och master i sync

Det är lite krånligt att hålla gh-pages och master i sync. Git är rätt så störande på det sättet.
Det här är det bästa sättet som jag har hittat.
Efter att du gjort ändringar i master-branchen går du in i gh-pages branchen och skriver:
```
git merge -X ours master
```
Så mergear och skriver över allting i gh-pages. Så var försiktig.
Sen behöver du inte ens adda å committa, det är bara att pusha å så är det klart.
Här är en del bra länkar.
Hur man skapar [gh-pages](https://help.github.com/articles/creating-project-pages-manually/) manuellt.
[http://classic.scottr.org/presentations/git-in-5-minutes/](http://classic.scottr.org/presentations/git-in-5-minutes/)

# Git workflow
Här är en riktigt bra beskrivning av hur ett bra workflow med git ska se ut.
https://guides.github.com/introduction/flow/index.html
