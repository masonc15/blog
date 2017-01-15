---
layout: post
title: Install Burp Suite on Ubuntu 14.04
date: 2016-03-17 19:19:21.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- burp suite
- how-to
- security
- tools
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '20857171450'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Burp Suite is a program that contains many features related to web security. It contains a proxy server that let's the user intercept and manipulate communication between the client/browser and the server. It lets the user manipulate data before it is sent to the server. It also contains a lot of other features.
In order to get Burp Suite up and running on Ubuntu 14.04 (and probably other Ubuntu-versions as well) we first need to make sure we have Java-installed. You can check that by running the following command in the terminal:
```
java -version
```

Your terminal should respond with the version, for example:
`java version "1.7.0_95"`
If you don't have java installed make sure to install it. [Here is a guide](https://www.digitalocean.com/community/tutorials/how-to-install-java-on-ubuntu-with-apt-get).
Okay, now go ahead and download Burp SUite from the [Portswiggers website](https://portswigger.net/burp/download.html).
Once it is downloaded you just move it to wherever you want to have it.
I had to change the file permission in order to execute it as a normal user.

```bash
chmod +x burpsuite_free_v1.6.32.jar
./burpsuite_free_v1.6.32.jar
```

## Configure with Chrome

In order to get burp suite to act as a proxy you need to configure your web browser, to pass all traffic through burp. In Firefox I think this is done through some settings in the browser, but Chrome picks up the settings from the computer itself. If you, in Chrome, go to `Preferences/Advanced Settings/Network/Change Proxy Settings` you will receive this note:
> When running Google Chrome under a supported desktop environment, the system proxy settings will be used.

However, either your system is not supported or there was a problem launching your system configuration.
But you can still configure via the command line. Please see man google-chrome-stable for more information on flags and environment variables."
So, in order to get the proxy set up correctly we have to open up System Settings, it can be done through the Unity Dash or with the command:
```
unity-control-center
```

In System Settings we open up Network, then we choose manual and add for HTTP Proxy 127.0.0.1, with port 8080.
Now you can go ahead and launch Burp Suite. Then click on Proxy and make sure that Intercept is on. Now you can visit any http-website. You will notice that the page will not load. That means that burp suite is working as it should, and that it has intercepted the http-request. Now we can change whatever parameters in the http-header that we want to change. And when you are done you just click Forward. If you don't want to intercept the traffic any more you just turn intercept off.
This is far from a perfect solution, but it works. But in order to use your browser without Burp Suite you have to turn off the Proxy in the System Settings/network.

## Configure with HTTPS
At the moment you are only able to intercept http-traffic. But these days less and less websites run without ssl. So let's configure Burp Suite to be able to intercept https traffic.

First go to `System Settings/Network/Network Proxy/Manual` there you add: 127.0.0.1 with port 8080 at the HTTPS-Proxy field.
Now open Burp Suite and go to Proxy and turn intercept on. Now go visit https://portswigger.net. You will now get an error message saying that the traffic is not secure. That is because Chrome knows that Burp is intercepting the traffic and it gives you a warning. For us it is no big deal because we know what is provoking it, but if you get that while you are surfing at a café or on some other open network you should assume that someone is monitoring your traffic.
Anyways, click on the little lock next to the cross-over HTTPS. Click on Certificate information, and then export the certificate so some place on your computer.
Now open up Preferences in Chrome and then scroll down to advanced settings, there you will find HTTPS/SSL Manage Exceptions. Click on it and the click on Authorities, then import. Then you just import the certificate and you are done.
Now Burp Suite should work on HTTPS-web pages.
## Running it on localhost
So, you want to try out Burp on a project that you are working on on your local computer. Burp does not initially work on localhost (your Internal IP-address). But that is easy to fix.
You just have to add it to your hosts file.
On ubuntu that means you have to do the following:

```bash
#Get your internal ip-address:
ifconfig
#Edit your hosts-file
sudo vim /etc/hosts
#Now you add your internal ip and give it a name.
192.168.1.123    myprojekt.dev
```

You can name it whatever you like. Remember that you have to have a tab between the ip and name. It cannot be spaces.
Now you just go to myprojekt.dev in your browser. If you are running it on a specific port just add the port in the browser, not in the hosts-file (because that won't work). So just go to: myprojekt.dev:8080 (or whatever port you use).
