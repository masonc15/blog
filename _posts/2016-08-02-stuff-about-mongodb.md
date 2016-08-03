---
layout: post
title: A few notes on MongoDB
---

I just want to mention how we can find out more about the queries we do in mongo. I usually use MongoChef and write these queries in the IntelliShell. But you can write it directly in the mongo-shell as well.

```javascript
db.user.find({active: true}).explain();
db.user.find({active: true}).explain("queryPlanner");
db.user.find({active: true}).explain("executionStats");
db.user.find({active: true}).explain("allPlansExecution");
```

The response may look something like this


```
"executionStats" : {
        "executionSuccess" : true,
        "nReturned" : NumberInt(13857),
        "executionTimeMillis" : NumberInt(15),
        "totalKeysExamined" : NumberInt(0),
        "totalDocsExamined" : NumberInt(28828),
        "executionStages" : {
            "stage" : "COLLSCAN",
            "filter" : {
                "active" : {
                    "$eq" : true
                }
            }
}
```

## Know when to use indexes
Indexes are not adequate at all occasions. For collections that change often it might not be optimal. Because the index needs to be rewritten every time.

## Difference between find() and findOne()
This is probably pretty obvious but still good to know. If you are looking for one element you use findOne() because after mongo finds that one element it will stop the search.
Find() however will continue the search throughout the whole collection, since there might be more documents with that condition.

## Details about MongoDB

Max-size for a document: 16 mb.
Max nested depth: 100 levels
Database names are *case incensitive*
