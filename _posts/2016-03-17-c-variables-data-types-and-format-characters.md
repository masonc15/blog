---
layout: post
title: C - variables, data-types and format-characters
date: 2016-03-17 02:42:03.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- c
- programming
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '20833599580'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Okay, so let's talk a bit about data-types in C. Coming from JavaScript this might be a bit confusing. In JavaScript you have few data-types (in comparison with C), six primitiv data-types: Null, Undefined, String, Number, Booleon, Symbol, and the data-type Object. JavaScript is a loosely-typed or dynamic language. This means that you don't have to define what type of data a variable will be.
For example, all of these data-types are different, but we just define them as variables, without specifying which types.

```javascript
var text = "I am a string";
var digit = 42;
var float = 4.2;
```

But in C that does not work.
So why do we need to define what type of variable/data-type we are going to use? Because that specifies the amount of space used in the storage, and how the bit-pattern is interpreted. So different data-types occupy different amount of storage. The minimum amount of memory we can manage in C is 1 byte.
Here are the different fundamental data-types and the space they take up (taken from [here](http://www.tutorialspoint.com/cprogramming/c_data_types.htm)):
**Character types**
```
char - 1 byte - -128 to 127 or 0 to 255
unsigned char - 1 byte - 0 to 255
signed char - 1 byte - -128 to 127
```

**Integer types**
```
int - 2 or 4 bytes - -32,768 to 32,767 or -2,147,483,648 to 2,147,483,647
unsigned int - 2 or 4 bytes - 0 to 65,535 or 0 to 4,294,967,295
short - 2 bytes - -32,768 to 32,767
unsigned short - 2 bytes - 0 to 65,535
long - 4 bytes - -2,147,483,648 to 2,147,483,647
unsigned long - 4 bytes - 0 to 4,294,967,295
```

**Float types**
```
float - 4 byte - 1.2E-38 to 3.4E+38 - 6 decimal places
double - 8 byte - 2.3E-308 to 1.7E+308 - 15 decimal places
# Because doubles take up double the space, 8 byte we can fit in more decimals in it.
long double - 10 byte - 3.4E-4932 to 1.1E+4932 - 19 decimal places
```
Well, that is a bit complicated. A better way to understand it to understand the following as basic variables:

```
int - integer.
float - decimal number.
double - more precise decimal number.
char - a single character.
void - valueless special purpose type.
```

These basic variables can be defined more precisely using `size qualifiers`, `sign qualifiers` or `const qualifier`.
The `size qualifier` alters the size of the variable by using the keywords `long` and `short`. The int variable is 2-4 bytes, but if long is used with it the size becomes 4-8 bytes.

int == 2-4 bytes
short int == 2 bytes
long int == 4-8 bytes
`Sign qualifiers` define if a variable can hold positive or negative values.
unsigned int;
Can only hold 0 or positive values.
A variable is by default signed. So that is not needed to add.

**Const qualifier**

A const keywords makes a variable constant, so that it can not be changed.
It's interesting to note here that C does not have the concept of strings. Just characters. A string in C is just an array of characters.
But in order to find out the size of a data-type or a variable you can use the built-in sizeof-function. Here is an example:

```c
#include <stdio.h>
#include <limits.h>
int main(){
  int test = 8;
  printf("Storage-size of a an int: %zu \n", sizeof(test));
  return 0;
}
```

So, the format to create variables in C is the following:
type name = value;

```c
#include <stdio.h>
int main(){
  // This is an integer
  // Format character: %d as in digit
  int age = 10;
  printf("This is the integer: %d\n", age);
  // This is a floating point
  // Format character: %f as in float
  float floating_point = 10.33;
  printf("This is the decimal_number %f\n", floating_point);
  // This is also a floating point, but much bigger.
  // Format character: also %f as in float
  double kinda_big_number = 44444.333233;
  printf("This is a big floating point: %f\n", kinda_big_number);
  // This is a character
  // Format character: %c as in character
  // Notice that a single character is written with only single-quotation-marks
  char one_character = 'H';
  printf("This is one character: %c\n", one_character);
  // This is a string
  // Format character: %s as in string
  char several_characters[] = "hello world";
  printf("This is a string: %s\n", several_characters);
  return 0;
}
```
