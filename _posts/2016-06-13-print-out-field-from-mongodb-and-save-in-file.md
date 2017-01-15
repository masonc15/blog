---
layout: post
title: Print out field from MongoDB and save in file
date: 2016-06-13 17:16:19.000000000 -04:00
type: post
published: true
status: publish
categories: []
tags:
- mongodb
- script
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '23796859731'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Today I encountered a fun little problem. I had to run through a database and print out every single field of the query and save it in a text document.
So I ended up doing it like this.
First I wrote a script that looks like this:
```javascript
//script.js
db.collection.find({active: true}).forEach(
    function(x) {
        print(x.id);
    }
);`
```
Then I ran the script like this:
```
mongo databaseName script.js | sed -n -e '1,1000p' > testing.txt
```
I added the sed-command to print out the first 1000 lines. This approach worked out well, and I can add sort and uniq if I need that.
