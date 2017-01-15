---
layout: post
title: MySQL Commands
date: 2016-04-02 22:48:01.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21392326586'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

This is a list of MySQL Commands that I don't feel like googeling again.
MySQL-commands are not case sensitive, so you can write it in lowercase if you like. But the commands are usually written in uppercase and  and tables, databases, usernames are written in lowercase.
All commands have to end in a semicolon. ;;;;;;;;;;;
**Create database**

```mysql
CREATE DATABASE nameofdatabase;
```

**Show database**

```
SHOW DATABASES;
```
**Delete database**

```
DROP DATABASE nameofdatabase;
```

**Open a database**

```
USE nameofdatabase;
```

**Show tables of a database**

```
SHOW tables;
```

**Create a table**
So when you create a table you have to give it at least one column, and that column need to have the data type specified. Here is a [list](http://www.tutorialspoint.com/mysql/mysql-data-types.htm) of common data types.

```
CREATE TABLE nameoftable (nameOfColumn TYPE, nameOfColumn TYPE);
```

For example:

```
CREATE TABLE users (username VARCHAR);
```

**Show a list of tables**

```
SHOW TABLES;
```

**Show a description of the table**

```
DESCRIPTION nameoftable;
```

**Insert data into table**

```
INSERT INTO `nameoftable` (`name`) VALUES ("Pelle");
```

**Look at the data in the table**

```
SELECT * FROM nameoftable;
```

**Update a field in the table**

```
UPDATE `users` SET `name` = 'Johan' WHERE `users`.`name` ='Pelle';
```

**Add column to a table**

```
ALTER TABLE users ADD email VARCHAR(40);
```

**Remove column from table**

```
ALTER TABLE users DROP email;
```

**Remove row from table**

Let's say we want to remove a user from our table

```
DELETE from users where name=Johan;
```
