---
layout: post
title: Compile and run a C program
date: 2016-03-16 22:08:11.000000000 -03:00
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
  _publicize_job_id: '20827819802'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

I have been programming in JavaScript now for a while, and I feel like it would be useful to learn a second language. One that is a bit closer to the machine, to learn a bit more about the computer. So I have settled on C.

C was developed in by Dennis Richie between 1969 and 1973, while he was working at AT&T Bell Labs. It was designed to be a general-purpose, high-level language that provide low-level access to memory. C is great because it produces efficient and fast programs. It can handle low-level activities, and do it almost as fast as assembly code. The language was developed closely together with the operating system Unix, and Unix was written in C. And it can be compiled on many different platforms. Linux OS, MySQL and NodeJS are some of the programs written in C. It is also the root of many other languages. Example of languages that have borrow from C are: C++, D, Go, Rust, Java, JavaScript, Limbo, LPC, C#, Objective-C, Perl, PHP, Python, Verilog.

Okay, so C seems like a useful language to learn. Let's get started.
The first thing we need to know is that in order to run C-code we first need to compile it. That mean we translate the code to an executable file, so that the computer understands it, and is able to run it. This basically mean that we translate the code to machine-code (assembly-code is basically the human-readable representation of machine-code) to be able to talk directly to the CPU. JavaScript is sometimes categorized as a interpreted language, but when used with for example Googles V8 Engine (that powers Node and Chrome) the JS-code is compiled straight to machine-code on the spot. It uses Just-in-time Compilation. The difference between V8 and Rheno (the mozilla JS-compiler) is that V8 does not produce any intermediate code, or bytecode. For more on the V8 click [here.](http://thibaultlaurens.github.io/javascript/2013/04/29/how-the-v8-engine-works/) Okay, so that was bit of a detour. Let's get back on track.
Unlike JavaScript that is run in Node we have quite a few different options on which compiler we want to to use to compile our C-code.
I am going to use the `make` program in linux. `Make` is a program that uses the **GCC** compiler. Even though the text CC pops up when you compile with make, CC usually points to GCC (Gnu compilation collection).
Anyways, let's get started to write and compile our first program.
hello-world.c

```c
#include <stdio.h>
int main()
{
    printf("hello, world\n");
    getchar();
    return 0;
}
```

Okay so we got the standard "hello world"-program written. Now it is time to compile and run it.

In the dir where you have your hello-world.c-file run the command:

```
make hello-world
```

Notice that we don't include the .c ending.
You will now have an executable know as hello-world, it can be run with the following command:
```
./hello-world
```

Let's look at the code.
First we do an `#include`. This is basically the same as a require in Node. We import the functions from the stdio.h module/library. stdio stands for Standard Input Output. So yeah, we gain access to the `printf`- (output), and `getchar`-function (input). This code is usually called preprocessor command.
The `int main` creates the main function, and we are saying that it will return an integer, hence the int. Every c-program includes a main function. The operating system that runs the program will first look for the main-function, and start there. So all other functions will derive from main.
`printf` is the standard print-function that prints to the console, and `getchar` takes input from the user through the console. In the end we return with 0. 0 mean success.
There is not that much new stuff coming from JS. We write the function code inside of curly-brackets, and we end with semi-colon. A major difference between JS and C here is that it is easy to break the C-code, just omit the semi-colon and it will throw an error, and it will not be able to compile, and you will not be able to execute your program.
