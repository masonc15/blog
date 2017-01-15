---
layout: post
title: CPU-intensive operations in NodeJS
date: 2016-07-22 13:46:44.000000000 -04:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _edit_last: '1959580'
  _oembed_8fa4285aae118b222d1ca07dd83e8760: "{{unknown}}"
  _oembed_754ad6998c5febdbd030fc6e7ef0613e: "{{unknown}}"
  _oembed_ded7199782ba9638c103f2dfdfd9aaf7: "<div class=\"embed-sitepoint\"><blockquote
    class=\"wp-embedded-content\"><a href=\"https://www.sitepoint.com/how-to-create-a-node-js-cluster-for-speeding-up-your-apps/\">How
    to Create a Node.js Cluster for Speeding Up Your Apps</a></blockquote><script
    type='text/javascript'><!--//--><![CDATA[//><!--\t\t!function(a,b){\"use strict\";function
    c(){if(!e){e=!0;var a,c,d,f,g=-1!==navigator.appVersion.indexOf(\"MSIE 10\"),h=!!navigator.userAgent.match(/Trident.*rv:11./),i=b.querySelectorAll(\"iframe.wp-embedded-content\"),j=b.querySelectorAll(\"blockquote.wp-embedded-content\");for(c=0;c<j.length;c++)j[c].style.display=\"none\";for(c=0;c<i.length;c++)if(d=i[c],d.style.display=\"\",!d.getAttribute(\"data-secret\")){if(f=Math.random().toString(36).substr(2,10),d.src+=\"#?secret=\"+f,d.setAttribute(\"data-secret\",f),g||h)a=d.cloneNode(!0),a.removeAttribute(\"security\"),d.parentNode.replaceChild(a,d)}else;}}var
    d=!1,e=!1;if(b.querySelector)if(a.addEventListener)d=!0;if(a.wp=a.wp||{},!a.wp.receiveEmbedMessage)if(a.wp.receiveEmbedMessage=function(c){var
    d=c.data;if(d.secret||d.message||d.value)if(!/[^a-zA-Z0-9]/.test(d.secret)){var
    e,f,g,h,i,j=b.querySelectorAll('iframe[data-secret=\"'+d.secret+'\"]'),k=b.querySelectorAll('blockquote[data-secret=\"'+d.secret+'\"]');for(e=0;e<k.length;e++)k[e].style.display=\"none\";for(e=0;e<j.length;e++)if(f=j[e],c.source===f.contentWindow){if(f.style.display=\"\",\"height\"===d.message){if(g=parseInt(d.value,10),g>1e3)g=1e3;else
    if(200>~~g)g=200;f.height=g}if(\"link\"===d.message)if(h=b.createElement(\"a\"),i=b.createElement(\"a\"),h.href=f.getAttribute(\"src\"),i.href=d.value,i.host===h.host)if(b.activeElement===f)a.top.location.href=d.value}else;}},d)a.addEventListener(\"message\",a.wp.receiveEmbedMessage,!1),b.addEventListener(\"DOMContentLoaded\",c,!1),a.addEventListener(\"load\",c,!1)}(window,document);//--><!]]></script><iframe
    sandbox=\"allow-scripts\" security=\"restricted\" src=\"https://www.sitepoint.com/how-to-create-a-node-js-cluster-for-speeding-up-your-apps/embed/\"
    width=\"600\" height=\"338\" title=\"Embedded WordPress Post\" frameborder=\"0\"
    marginwidth=\"0\" marginheight=\"0\" scrolling=\"no\" class=\"wp-embedded-content\"></iframe></div>"
  _oembed_time_ded7199782ba9638c103f2dfdfd9aaf7: '1469136074'
  _oembed_868d35feffb79647c43bcd812bc867bb: "{{unknown}}"
  _oembed_c5e01c5829fe8d5618fed65bf87557f4: "{{unknown}}"
  _oembed_4d286495fab0546a994f09102a58035f: "{{unknown}}"
  _oembed_03985034ab305d7dd0cf34e4c4c67ae8: "{{unknown}}"
  _oembed_c0b3729ce40f80baf320a583dacd3b12: "{{unknown}}"
  _oembed_513232c7a9e90605cab9ad3321ae9d1a: "{{unknown}}"
  _oembed_27d47e54ecf0cb32533ced389950aafe: "{{unknown}}"
  _oembed_0878593a755d108c528d8e47f30dfe50: "{{unknown}}"
  _oembed_a16bd23cd10b7560b5434bc054fc4604: "<div class=\"embed-sitepoint\"><blockquote
    class=\"wp-embedded-content\"><a href=\"https://www.sitepoint.com/how-to-create-a-node-js-cluster-for-speeding-up-your-apps/\">How
    to Create a Node.js Cluster for Speeding Up Your Apps</a></blockquote><script
    type='text/javascript'><!--//--><![CDATA[//><!--\t\t!function(a,b){\"use strict\";function
    c(){if(!e){e=!0;var a,c,d,f,g=-1!==navigator.appVersion.indexOf(\"MSIE 10\"),h=!!navigator.userAgent.match(/Trident.*rv:11./),i=b.querySelectorAll(\"iframe.wp-embedded-content\"),j=b.querySelectorAll(\"blockquote.wp-embedded-content\");for(c=0;c<j.length;c++)j[c].style.display=\"none\";for(c=0;c<i.length;c++)if(d=i[c],d.style.display=\"\",!d.getAttribute(\"data-secret\")){if(f=Math.random().toString(36).substr(2,10),d.src+=\"#?secret=\"+f,d.setAttribute(\"data-secret\",f),g||h)a=d.cloneNode(!0),a.removeAttribute(\"security\"),d.parentNode.replaceChild(a,d)}else;}}var
    d=!1,e=!1;if(b.querySelector)if(a.addEventListener)d=!0;if(a.wp=a.wp||{},!a.wp.receiveEmbedMessage)if(a.wp.receiveEmbedMessage=function(c){var
    d=c.data;if(d.secret||d.message||d.value)if(!/[^a-zA-Z0-9]/.test(d.secret)){var
    e,f,g,h,i,j=b.querySelectorAll('iframe[data-secret=\"'+d.secret+'\"]'),k=b.querySelectorAll('blockquote[data-secret=\"'+d.secret+'\"]');for(e=0;e<k.length;e++)k[e].style.display=\"none\";for(e=0;e<j.length;e++)if(f=j[e],c.source===f.contentWindow){if(f.style.display=\"\",\"height\"===d.message){if(g=parseInt(d.value,10),g>1e3)g=1e3;else
    if(200>~~g)g=200;f.height=g}if(\"link\"===d.message)if(h=b.createElement(\"a\"),i=b.createElement(\"a\"),h.href=f.getAttribute(\"src\"),i.href=d.value,i.host===h.host)if(b.activeElement===f)a.top.location.href=d.value}else;}},d)a.addEventListener(\"message\",a.wp.receiveEmbedMessage,!1),b.addEventListener(\"DOMContentLoaded\",c,!1),a.addEventListener(\"load\",c,!1)}(window,document);//--><!]]></script><iframe
    sandbox=\"allow-scripts\" security=\"restricted\" src=\"https://www.sitepoint.com/how-to-create-a-node-js-cluster-for-speeding-up-your-apps/embed/\"
    width=\"600\" height=\"338\" title=\"Embedded WordPress Post\" frameborder=\"0\"
    marginwidth=\"0\" marginheight=\"0\" scrolling=\"no\" class=\"wp-embedded-content\"></iframe></div>"
  _oembed_d94b6466b12703af4cac470b747ab0ed: "<div class=\"embed-sitepoint\"><blockquote
    class=\"wp-embedded-content\"><a href=\"https://www.sitepoint.com/how-to-create-a-node-js-cluster-for-speeding-up-your-apps/\">How
    to Create a Node.js Cluster for Speeding Up Your Apps</a></blockquote><script
    type='text/javascript'><!--//--><![CDATA[//><!--\t\t!function(a,b){\"use strict\";function
    c(){if(!e){e=!0;var a,c,d,f,g=-1!==navigator.appVersion.indexOf(\"MSIE 10\"),h=!!navigator.userAgent.match(/Trident.*rv:11./),i=b.querySelectorAll(\"iframe.wp-embedded-content\"),j=b.querySelectorAll(\"blockquote.wp-embedded-content\");for(c=0;c<j.length;c++)j[c].style.display=\"none\";for(c=0;c<i.length;c++)if(d=i[c],d.style.display=\"\",!d.getAttribute(\"data-secret\")){if(f=Math.random().toString(36).substr(2,10),d.src+=\"#?secret=\"+f,d.setAttribute(\"data-secret\",f),g||h)a=d.cloneNode(!0),a.removeAttribute(\"security\"),d.parentNode.replaceChild(a,d)}else;}}var
    d=!1,e=!1;if(b.querySelector)if(a.addEventListener)d=!0;if(a.wp=a.wp||{},!a.wp.receiveEmbedMessage)if(a.wp.receiveEmbedMessage=function(c){var
    d=c.data;if(d.secret||d.message||d.value)if(!/[^a-zA-Z0-9]/.test(d.secret)){var
    e,f,g,h,i,j=b.querySelectorAll('iframe[data-secret=\"'+d.secret+'\"]'),k=b.querySelectorAll('blockquote[data-secret=\"'+d.secret+'\"]');for(e=0;e<k.length;e++)k[e].style.display=\"none\";for(e=0;e<j.length;e++)if(f=j[e],c.source===f.contentWindow){if(f.style.display=\"\",\"height\"===d.message){if(g=parseInt(d.value,10),g>1e3)g=1e3;else
    if(200>~~g)g=200;f.height=g}if(\"link\"===d.message)if(h=b.createElement(\"a\"),i=b.createElement(\"a\"),h.href=f.getAttribute(\"src\"),i.href=d.value,i.host===h.host)if(b.activeElement===f)a.top.location.href=d.value}else;}},d)a.addEventListener(\"message\",a.wp.receiveEmbedMessage,!1),b.addEventListener(\"DOMContentLoaded\",c,!1),a.addEventListener(\"load\",c,!1)}(window,document);//--><!]]></script><iframe
    sandbox=\"allow-scripts\" security=\"restricted\" src=\"https://www.sitepoint.com/how-to-create-a-node-js-cluster-for-speeding-up-your-apps/embed/\"
    width=\"500\" height=\"282\" title=\"Embedded WordPress Post\" frameborder=\"0\"
    marginwidth=\"0\" marginheight=\"0\" scrolling=\"no\" class=\"wp-embedded-content\"></iframe></div>"
  _oembed_time_d94b6466b12703af4cac470b747ab0ed: '1469195208'
  _oembed_03c632a070440b4ae0b4e25c22a3a7de: "{{unknown}}"
  _oembed_0e1b1141b93359f5e5bfd2c3819cb814: "{{unknown}}"
  _oembed_01f16a46935d24f4865468b4573cefd0: "{{unknown}}"
  _rest_api_published: '1'
  _oembed_62353862461d0191c613136f7e266087: "{{unknown}}"
  _rest_api_client_id: "-1"
  _publicize_job_id: '25046936229'
  _oembed_3956c49e293129d7c1ad5dc2f0af4f74: "{{unknown}}"
  _oembed_time_a16bd23cd10b7560b5434bc054fc4604: '1469195213'
  _oembed_374ee90f1c32dbc5bb1b10e9b4446719: "{{unknown}}"
  _oembed_02a6e331faa86b91cee9685498379883: "{{unknown}}"
  _oembed_953ac011596f7b4dce9e83fb364cb2e3: "{{unknown}}"
  _oembed_0505cd98bad14e677874fefcfb63e98e: "{{unknown}}"
  _oembed_00384edf58a66944d737db93624a36ec: "{{unknown}}"
  _oembed_6894d331a4a82185e988137d2bb01505: "{{unknown}}"
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Lately I have been learning about CPU-intensive operations and I/O-intensive operations. The problem I have run into has been: how do I handle long-running tasks in the routes/endpoints. Let's say that you have a million items you want to show the user, so and you have to iterate over all the items and edit them in some way. Wait, that a crappy example, let's instead say that you have a ton of statistics and you want to give the user a real-time analysis of that statistics. That is going to take a lot of time. If that happens right in the endpoint that request is going to block the node-server until the process is finished. That's because of the single-threadedness of node ([Yada yada this presentation is still the best thing on the internet to explain nodes event-loop.](https://www.youtube.com/watch?v=8aGhZQkoFbQ)). This means that all other users have to wait for that request to finish.
While I read about this I came across a few concepts that are useful to know.

## CPU-bound
This can sometimes be called: long running CPU tasks, CPU intensive tasks or
CPU-bound tasks.
A program/application/process that is CPU-bound is something that requires a lot of CPU-power. CPU-power is usually needed when doing a lot of calculations.

## I/O-bound
An application that is I/O-bound is mostly wait for network or database. This is something that Nodejs does really good. Since it can process several requests while it is waiting for a response from the database. That's why node can be so fast in servering many requests.

## Different Server
So there are conceptually some different servers. One being a web-server.
A web-server is basically a I/O-bound server. The main focus is to server requests as fast as possible. That mean that if you have an operation that is CPU-bound is does not belong in the Web-server. That process probably belongs better in a worker-server.

## Possible solutions for CPU-bound operations in a NodeJs-server
Okay, so how to we solve the problem of doing CPU-bound operations on a NodeJS application?
Here are some of the alternatives I have run into while researching this. This list is not exhaustive, and it might not even be correct (I have just been googeling around a bit, I'm no expert).

1. "Offline"-server dedicated to processing data
You can have a server that process the data offline, in the sense that it is not in real-time.

  A. Cron-job
  Having a cron-job that calculates on a schedule. For example every half hour or something like that. This is of course very resource consuming. What if nothing has happened and the server process the data again? That would be very unnecessary.

  B. Only calculate when an update in the database is done. This could be achieved by having a message sent every time the database gets updated.

  C. Do part of the calculation when an update to the database is done. But only process the data since the last process. If this is possible.
  There is a pretty good answer on [SO about this.](http://programmers.stackexchange.com/questions/158781/cpu-heavy-web-server)

2. Separate server do the CPU-crunching.
Have a separate server do the number crunching. And then just connect the web-server to that endpoint. Since the request is async, other requests will work meanwhile. Since node can work while waiting for a callback.
Here is a [blog-post discussing this.](http://davidherron.com/blog/2014-07-03/easily-offload-your-cpu-intensive-nodejs-code-simple-express-based-rest-server)

3. Using Nodes ClusterModule -- Clusters - LoadBalancer with separate processes
https://www.sitepoint.com/how-to-create-a-node-js-cluster-for-speeding-up-your-apps/

4. Rabbit queue message

5. process.NextTick/setImmediate
If you know what you are doing and you really want the operation to happen on your web-server. This can be a way.
This trick can make synchronous operations asynchronous. So that the server can attend other easier tasks meanwhile. This is [blog](http://neilk.net/blog/2013/04/30/why-you-should-use-nodejs-for-CPU-bound-tasks/) exemplified it pretty well.

http://bytearcher.com/articles/io-vs-cpu-bound/?utm_source=hashnode.com
http://stackoverflow.com/questions/3491811/node-js-and-cpu-intensive-requests?rq=1
http://stackoverflow.com/questions/16974557/why-is-node-js-not-suitable-for-heavy-cpu-apps?noredirect=1&amp;lq=1
http://programmers.stackexchange.com/questions/158781/cpu-heavy-web-server
https://www.toptal.com/nodejs/top-10-common-nodejs-developer-mistakes
http://neilk.net/blog/2013/04/30/why-you-should-use-nodejs-for-CPU-bound-tasks/
