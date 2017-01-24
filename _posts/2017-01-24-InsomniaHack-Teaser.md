---
layout: post
title: InsomniHack Teaser - The Great Escape Part 1 - Writeup
---

I did one challenge from the recent InsomniHack Teaser CTF. It was the first CTF I have participated in and it was a great experience. This year I am going to try to participate in as many CTFs as I can. I think my plan is to try to learn a little bit about all the categories, so that I can at least do the basics of all categories and then try to specialize in one. 

## The Great Escape Part 1

This was the challenge description:

**Hello,**

**We've been suspecting Swiss Secure Cloud of secretely doing some pretty advanced research in artifical intelligence and this has recently been confirmed by the fact that one of their AIs seems to have escaped from their premises and has gone rogue. We have no idea whether this poses a threat or not and we need you to investigate what is going on.**

**Luckily, we have a spy inside SSC and they were able to intercept some *communications* over the past week when the breach occured. Maybe you can find some information related to the breach and recover the rogue AI.**

**X**

There was a .pcapng file included. So I opened up the file in wireshark and started looking around. First I sorted the packets based on protocol to get an overview of what type of traffic was captured. I found traffic using these protocols: **ftp**, **tcp**, **http**, **imf**, **ocsp**, **smtp**, and **TLSv1**. The traffic using TLS was of course encrypted so I was not able to read much of that.

I started looking at the FTP-traffic. So I right-clicked on one of the ftp-packets and did `Follow TCP stream`. Which returned this:

![FTP-login]({{ site.url }}{{ site.posturl }}/assets/insomni-hack-ctf/ftp-login.png)

Here we can clearly see that bob has logged in and used the `STOR` command. `STOR` is a protocol command, and not an application command. In an application such as `ftp` or `ftp.exe` you can use the `PUT` command. `PUT` is just an abstraction of `STOR`. You can read more about `STOR` in the [FTP-RFC](https://tools.ietf.org/html/rfc959). The file that was stored was called `ssc.key`. So that looked very interesting. 

The `ssc.key`-data was not being transferred on the standard ftp port 21, but instead the FTP-server momentarily opens up port 20 and transfers the data on that port. That is why wireshark defines the protocol for the data-transfer as `FTP-data` and not just `FTP`. 

![FTP-Data]({{ site.url }}{{ site.posturl }}/assets/insomni-hack-ctf/ftp-data.png)

So I followed the TCP-stream for that packet and got a private key. I continued to look around the pcap to see what else I could find. I followed the TCP-stream for the packets that were sent using SMTP. 

![SMTP-mail]({{ site.url }}{{ site.posturl }}/assets/insomni-hack-ctf/smtp-mail.png)
 
I visited https://ssc.teaser.insomnihack.ch and looked around, trying to figure out where the ssc.key would go. Although the ssc.teaser.insomnihack.ch looked promising, since it creates a public and private key that gets stored in the browsers Local storage, it did not seem like the key would fit.

Instead I studied up on how to use a key to decrypt encrypted traffic. It was a lot easier than some tutorials out there made it out to be.


First I saved the key in a file called `cert.pem`. Then I went to **Edit/Preferences/Protocol/SSL**. Clicked on `RSA Keys List - Edit`. Added the following configurations:

![SSL]({{ site.url }}{{ site.posturl }}/assets/insomni-hack-ctf/insomni-ssl.png)

Then I was able to filter out HTTP-traffic, which now showed the unencrypted traffic, and in the header I found the flag.


![Flag]({{ site.url }}{{ site.posturl }}/assets/insomni-hack-ctf/flag.png)


