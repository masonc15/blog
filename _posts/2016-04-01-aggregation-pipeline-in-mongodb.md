---
layout: post
title: Aggregation pipeline in MongoDB
date: 2016-04-01 13:04:39.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21346315206'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Aggregate is a way to find and filter data in MongoDB.
It uses a pipeline structure, so the data is requested and then pass through a number of different filters. Is is similar to piping as we do it in unix.
The one I have been using is $match and $group

## $group
Group is one of the most common methods in aggregate. It takes the input data and divides it up in different buckets/sections (or whatever you like to call it).
Let's say our database looks something like this:
```javascript
//collection-name: names
{
nombre: "Philip"
},
{
nombre: "Pelle"
},
{
nombre: "Philip"
}
```
It is just three documents with one value, nombre. So lets group these values together.

```javascript
    names.aggregate({
      $group: {
        _id: {
          name: "$nombre"
        }
      }
    }, function(err, result){
      console.log(result)
    });
```

This pipes the whole collection through our aggregate method. The method then groups the different $nombre-fields into different buckets. All the fields with "Philip" is grouped together in one bucket. And "Pelle" is put into it's own bucket.
This is not so useful though. So let's count to see how many Philip's there are, and how many Pelle. This introduces at least a little bit of functionality.

```javascript
      $group: {
        _id: {
          name: "$nombre"
        },
        count: {
          $sum: 1
        }
      }
```

So now we have created a bucket for each nombre-field in the documents. Now we want to count how many they are. So in the group object we create a count-key and as a value we add another object that sums it all up, adding 1 for each document. If we but 2 it is going to add two for each document. Pretty simple.
So now, we are gonna get the data without any order. So let's sort it in ascending order.

```javascript
    name.aggregate({
      $group: {
        _id: {
          name: "$nombre"
        },
        count: {
          $sum: 1
        }
      }
    },
    {
      $sort: {
        count: -1
      }
    }, function(err, result){
      console.log(result)
    });
```
Okay, so here we are saying that we want to sort the count-variable and then sort it in ascending order.
That's it for now.
