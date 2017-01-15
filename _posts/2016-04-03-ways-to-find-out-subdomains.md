---
layout: post
title: Ways to find out subdomains
date: 2016-04-03 18:08:29.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- pen-testing
- security
meta:
  _oembed_0d724ec151d31f3b1c91e9b192f7b692: "{{unknown}}"
  _oembed_230e6e5458de0416b0ac5641d2e6fe8a: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '21415941538'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

There are a few different ways to find out subdomains of a specific site. None of them can guarantee that you will find all subdomains. So you will probably have to use several different tools.

**Google Dorks**

You can use google. With this type of search you will find some of the
site:*.example.com/ -site:www.example.com

**Online tools**

[Dnsdumpster](https://dnsdumpster.com/)

[Pentest-tools.com](https://pentest-tools.com/information-gathering/find-subdomains-of-domain)

**SubBrute**
SubBrute is a python-program that bruteforces the subdomains. It can be quite useful.
[Github repo](https://github.com/TheRook/subbrute)
