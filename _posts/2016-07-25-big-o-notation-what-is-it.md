---
layout: post
title: Big-O notation - what is it? How does it work?
date: 2016-07-25 23:57:36.000000000 -04:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _oembed_16cf4bb63e1b0567be14d694515748b8: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '25152729265'
  _oembed_e24aab004cd6b8a4652e8b2e8bc46c78: "{{unknown}}"
  _oembed_9f994cdadbb1ccf92d865672470e5e76: "{{unknown}}"
  _oembed_cf114e3c0a23ca056836b22b507f4662: "{{unknown}}"
  _oembed_3b2fde29c5a0ecda03ab319f6bebbd8d: "{{unknown}}"
  _oembed_0368fb70a887463560c883a34786d2b8: "{{unknown}}"
  _oembed_06452fc8201dffeaca1cf9e84a36bea2: "{{unknown}}"
  _oembed_68a7be2c041bbe8bc1fd9da1f2d7ea96: "{{unknown}}"
  _oembed_1170b795e7dc7ead6bac557bfef0cf9c: "{{unknown}}"
  _oembed_f00c85c8c57ee8be2d9101982f85bbe7: "{{unknown}}"
  _oembed_42ad758959d70fb338b2b6b1cb81da2f: "{{unknown}}"
  _oembed_6994c4bb5b2e20945794ef749dc04336: "{{unknown}}"
  _oembed_86197eafc897664d7766a10bcb82114b: "{{unknown}}"
  _oembed_23282be30d6aa7ce76ef9456bdaafcdb: "{{unknown}}"
  _oembed_9758b7b43199a713d00c6d77b311eac9: "{{unknown}}"
author:
  login: tidlost
  email: philip.linghammar@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---


I think it is like a rite of passage for a self-taught programmer to write this blog post. You will get to a point where you feel inadequate for not understanding the Big-O notation. You think it is some dark magic that they only teach at computer science programs at the university. Okay, so let's get to the bottom of this, well at least get through the basics.
First, Big-O notation helps us understand the complexity of an algorithm. And it always looks at the worst-case-scenario of an algorithm, the slowest possible outcome.
I am going to go through these types. In more practical terms, we are looking at understanding CPU-usage.
O(1) - Order of one
O(N) - Order of N
O(N + M)
O(N * M)
O(N^2) - Order of N squared
O(log N) - Order of log N
O(n log N) - Order of n log N

### O(1) - Constant-time algorithm
This is just a simple operation that takes the same amount of time to do no matter how big the number is. This is also called **Constant-time algorithm.**
For example

```javascript
var myArray = [];
//This will take as much time to perform as:
console.time("n(1)");
myArray.push(10);
console.timeEnd("n(1)");
// this
console.time("n(1)");
myArray.push(1000000000000000000000000000000000000000000000000);
console.timeEnd("n(1)");
```

The complexity of this operation will never change no matter how big the number we push is. The push is only one operation, no matter how big the number is. This is usually represented as a flat line in an x/y diagram. It is the blue line in the image. As you can see the complexity of the algorithm doesn't increase if we increase the amount of data.
<img class="alignnone size-full wp-image-1843" src="{{ site.baseurl }}/assets/bigo.png" alt="bigO" width="526" height="378" />

### O(N) - Linear-time algorithm
So let's move on to the next on our list. So in this case the complexity is directly related to the size of the array we want to iterate over. Let's look at an example.

```javascript
console.time("O(N)");
for (var i = 0; i < 1e7; i++) {
}
console.timeEnd("O(N)");
console.time("O(N)");
for (var i = 0; i < 2e7; i++) {
}
console.timeEnd("O(N)");
console.time("O(N)");
for (var i = 0; i < 3e7; i++) {
}
console.timeEnd("O(N)");
console.time("O(N)");
for (var i = 0; i < 4e7; i++) {
}
console.timeEnd("O(N)");
//O(N): 13.415ms
//O(N): 21.307ms
//O(N): 30.151ms
//O(N): 39.580ms
```

As we can see here it takes roughly nine milliseconds for each 10 million loop. This means that for every 10 million items we add to the loop the algorithm complexity increase will be linear. As you can see on the image above.
A more practical example would be. If you need to search through an array to find a specific item you need to iterate over every single item. Of course it could be that you find the number you are looking for after one iteration, it might be in the beginning of the array. But Big O notation assumes **worst case scenario**.

### O(N+M)

Well, two loops iterating over their own dataset.

```javascript
var n = 10;
var m = 100;
for (var i = 0; i < n; i++) {
//Do stuff
}
for (var i = 0; i < m; i++) {
//Do stuff
}
```

### O(N^2) - Quadratic-time algorithm

So far so good, nothing to complicated. Now let's look at O(N^2).
Let's say you are given an array with numbers. You are then asked to find out if it is possible to get the number 10 by adding two of the numbers in the array together. To do this you could write the following code.

```javascript
function findSum(arr, sum){
  for(var i = 0; i < arr.length - 1; i++){
    for(var j = i + 1; j < arr.length; j++){
      if (arr[i] + arr[j] == sum)
    return true;
    }
  }
 return false;
}
```

This is like a brute-force attempt. We are just trying every single possible combination until we get a success. This is not very efficient, but it works. And if the size of the array increases the run time complexity will increase exponentially. If there are five items in the array it must loop through it `5^2`. This can pretty soon generate code that needs to do so many iterations. By increasing it from **5^2 (25)** to `6^2 (36)` we just increase the array with one (one more item in the array), but we increase the number of iterations by `36 - 25 = 11`.

A practical example of this would be the bubble sort.
If you have three loops nested we get `O(N^3)`, and so on. This is course gets slow fast, so if you notice you have a `O(N^2)` or a `O(N^3)` algorithm you should probably think of ways to fix it.

To pronounce this you say **"big oh of n squared"**.

### O(N*M)
Not all nested loops are  `O(N^2)`. For example, the following would not be `O(N^2)`.

```javascript
var arrayOne = 10;
var arrayTwo = 100;
for (var i = 0; i < arrayOne.length; i++) {
for (var j = 0; j < arrayTwo.length; j++) {
}
}
```

Since we are iterating over another loop inside the first loop we are not looping `n^2`, but instead we are looping `n*m`.

### O(log N) - Logarithmic

Log is the shorthand for logarithm. And logarithm is the opposite of exponential. A concept I am more familiar with, so it helps me to understand logarithm.
This is a great description.
`O(log n)` can be seen as: If you double the problem size n, your algorithm needs only a constant number of steps more.

Image
<img src="{{ site.baseurl }}/assets/Log_Exp_inverts.jpg" />

Okay, this might freak someone out. Since it look way to math-ish. So instead let's look at a video. Minute 17-25: [CS50 - Week 00](https://www.youtube.com/watch?v=zFenJJtAEzE)

This is an amazing explanation:
https://stackoverflow.com/questions/9152890/what-would-cause-an-algorithm-to-have-olog-n-complexity

A good example of `O(log N)` is binary tree search.
Example:
Finding a number in an array, the Binary Tree Search-way. Or the `O(log N)`-way.

#### References

This is a great reference to algorithms that state their complexity with big-o.
http://www.thatjsdude.com/interview/js1.html

http://discrete.gr/complexity/

Nested loops
https://stackoverflow.com/questions/362059/what-is-the-big-o-of-a-nested-loop-where-number-of-iterations-in-the-inner-loop
https://www.youtube.com/watch?v=V6mKVRU1evU
https://justin.abrah.ms/computer-science/big-o-notation-explained.html
