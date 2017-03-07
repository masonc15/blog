---
layout: post
title: PragYan CTF - Writeup
---

Did a few challenges on the PragYan CTF this weekend. Most of the challenges were with steganography and crypto. Even challenges found in other categories. 

## Look Harder

> There are rumours that in the Great Sahara Desert, a great treasure has been buried deep inside the ground, but the map for the exact location of the treasure over the years, has not been preserved properly. You have got hold of the map, but it looks nothing more than a plain white sheet of paper. Can you make sense out of it ??

We are given an image that is completely white.


![Result]({{ site.url }}{{ site.posturl }}/assets/pragyanctf/treasure_map.png)


The easy way to solve this is to just open it up in stegsolve.jar and then click through the different filters. Gray bits will show a QR-code. I solved this in just a few minutes. But I didn't really understand how it actually worked. How can a completely white image be hiding a QR-code? So I set out to investigate a little bit.

What is interesting about this image is that it is very small in size. It is only 449 bytes.

We can verify this using stat `stat treasure_map.png` or `ls -lash treasure_map.png`. So why is it so small? 

If we run `file treasure_map.png` we get this output:

```
treasure_map.png: PNG image data, 400 x 400, 1-bit colormap, non-interlaced
```

So it looks like it is a 1-bit colormap. That means that each pixel only contains one bit, so each pixel can only contain two colors. Or you can say that each pixel is represented with one bit. These colors are often black or white, but could be any other color. Normally it is 0 for black and 1 for white. [Here](http://paulbourke.net/dataformats/bitmaps/) and [here](https://en.wikipedia.org/wiki/Binary_image) are some good articles explaining this.

A black-and-white (monochrome) image has 1 bit colormap, gray-scale has an 8-bit colormap, and rgb has 24 bit colormap (8 bits for each color, red, green and blue), and so on.

I wanted to confirm that each pixel was represented by just one bit. And I have been playing around with the python module pillow lately so I figured this would be a good time to use it. So I simply iterated over each pixel and counted the values.

```python
from PIL import Image

im = Image.open("treasure_map.png")

white = 0
black = 0

for row in range(im.width):
  for col in range(im.height):
    pixel = im.getpixel((row,col))
    if pixel == 1:
      white = white + 1
    else: 
      black = black + 1

print "White: ", white
print "Black: ", black
print "Total: ", white + black
```

And the result was:

```
White:  70313
Black:  89687
Total:  160000
```

Okay, so now we know that there is something hiding in the image. But why is it still just white?

If we look at the color palette that the image use we can see that it only uses two colors, which make sense since it is a 1 bit colormap. But those two colors are both white. So all we really have to do is create a new palette that includes black and then set the colormap to that palette.

So using Gimp we can create a new palette with the colors black and white. And then we go to `Colors > Map > Set Colormap > Palette > BlackAndWhite`.

And now we can see the result:

![Result]({{ site.url }}{{ site.posturl }}/assets/pragyanctf/result.png)


The QR-code can be uploaded here: [https://webqr.com/](https://webqr.com/)

The flag: {stay_pragyaned}



## Interstellar

> Dr. Cooper, on another one of his endless journeys encounter a mysterious planet. However when he tried to land on it, the ship gave way and he was left stranded on the planet. Desperate for help, he relays a message to the mothership containing the details of the people with him. Their HyperPhotonic transmission is 10 times the speed of light, so there is no delay in the message. However, a few photons and magnetic particles interefered with the transmission, causing it to become as shown in the picture. Can you help the scientists on the mothership get back the original image?

Here is the image that we were given: 


![Transmission]({{ site.url }}{{ site.posturl }}/assets/pragyanctf/transmission.png)


Interstellar is another steganography-challenge. Which for some reason is put in the forensics section. It can easily be solved with stegsolve.jar using the xor-filter, or using some other filters that work.

But I wanted to dig a little bit deeper into how this works. We can start by looking at what type of file it is `file transmission.png`

```
transmission.png: PNG image data, 1920 x 1080, 8-bit/color RGBA, non-interlaced
```

So this image has an 8-bit color depth. With the colors red, green, blue and the alpha channel. This means that each pixel needs be defined with these colors. 

When we open up the image in Gimp we see that a large part of the image has a checkered background. That means that the opacity level is very high at that area, or the alpha-level is very low. So what we can do is simple remove the opacity/alpha level of the image. I did this in Gimp like this `Colors > Levels > Channel > Alpha > Output Levels 255 - 255`.

This can also be done programatically. We simply convert the file from a RGBA to a RGB. 

```python
from PIL import Image


im = Image.open("transmission.png")

print "Showing RGBA channels: "
print im.getpixel((1000, 500))

im = im.convert("RGB")

print "Showing RGB channels: "
print im.getpixel((1000, 500))

im.save("transsmission-solved.png")
```

With this output:

```
# python remove_alpha.py

# Red, Green, Blue, Alpha
(125, 133, 136, 0)

# Red, green, blue
(125, 133, 136)
```

So this is a pretty easy way to do it. But we can also do it in an insanely inefficient way by iterating over each pixel and setting the alpha to 255.

```python
from PIL import Image

im = Image.open("transmission.png")


for row in xrange(im.width):
  for col in xrange(im.height):
    pixel = im.getpixel((row,col))
    pixel = list(pixel)
    pixel[3] = 255
    pixel = tuple(pixel)
    im.putpixel((row,col), pixel)


im.save("Transsmission-slow.png")
```


![Transmission-solved]({{ site.url }}{{ site.posturl }}/assets/pragyanctf/transmission-solved.png)

Anyways, here we have the flag.
The flag is: {Cooper_Brand}


## Answer to everything


> Shal has got a binary. It contains the name of a wise man and his flag. He is unable to solve it.

> Submit the flag to unlock the secrets of the universe.
main.exe



So I started by just looking at what type of file it was.

```
file main.exe
main.exe: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=4b9b47b7eac612e0c367f0e3a9878eb1f09b841d, not stripped
```

By just running strings we could figure this one out pretty fast:

```

--snip--

Cipher from Bill 
Submit without any tags
#kdudpeh
YOUSUCK
Gimme: 

--snip--

```

The `#kdudpeh` looks pretty suspicious.

But let's have a look at the code instead and see what we can find. 

![Code]({{ site.url }}{{ site.posturl }}/assets/pragyanctf/not_flag.png)

First we just follow the execution into the function `not_the_flag`. There we can see that it makes a comparison between `0x2a` and whatever is on the stack at that point (`rbp-0x4`). `0x2a` is `42` in decimal. Which seems correct, since it seems to fit with the reference to The Hitchhikers Guide to the Galaxy in the title of the challenge.

So when we rerun the binary and input 42 we get this back:

```
Submit without any tags
#kdudpeh
```

So I checked [here](https://kt.pe/tools.html#conv/%23kdudpeh) if it could be a a rot-something, and it was a rot23 for `#harambe`.

Then I felt like I should write my own little rot-program. So I did that. It only outputs the result in lower-case though. So that is a bit of a limit. Here is the code:

```python
import sys

message = sys.argv[1].lower()
splitted = list(message)

for num in range(26):
  result = []
  for char in splitted:
    if char.isalpha():
      charNum = ord(char)
      # 122 because that is "z" in the ascii-table
      if charNum + num > 122:
        # 96 because that is "a" in the ascii-table
        char = chr(96 + ((charNum + num) - 122))
        result.append(char)
      else :
        result.append(chr(charNum + num))
    else:
      result.append(char)
  
  print "Rot" + str(num) +": " + "".join(result)
```

And the flag is: pragyanctf(harambe)

## Roller Coaster Ride

> Bobby has been into Reverse Engineering and Binary Exploitation lately.
One day, he went to an amusement park in his city. It was very famouse for its Roller Coaster Rides.
But, Bobby, being 12 years old, was not allowed on those rides, as it was open for people who were 14 years or older.
This made Bobby very angry. On reaching home, he hacked into the servers of the amusement park, got hold of the validation software for the Roller Coaster rides, and modified it, so that nobody is allowed to have a ride on those Roller Coasters.

> The authorities were in trouble now, as they weren't able to allow access to the Roller Coaster rides, without a Go-Ahead from their validation software.
They have come to you, asking for your help to fix their issue. Can you help them with it ??

A long introduction. So basically we should just look around a bit and see what happens.

```
file validation

validation: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.24, BuildID[sha1]=f18f0acc149e2330b7549976f9e25c1b4e97e4f8, not stripped
```

So it looks like it is an ELF 64-bit executable. 


First I ran `objdump -d validation -M intel` to get a feel for it. From it I could see that it contains a function called `verify_your_name`, `main` and functions named `f1` up to `f9`.


So I went through the code in gdb to see what is happening.

First we get redirected to `verify_your_name`. That function then calls the printf and scanf that asks for the users name and age. After that the f1 function is called. And in that function, and in all the other f-functions we see code similar to this:

```
=> 0x40075b <f6+1>: mov    r9,0x1a
   0x400762 <f6+8>: mov    rax,0x68
   0x400769 <f6+15>:  xor    rax,r9
```

What this does is that it move the immediate value `0x1a` to register `r9`. Then move `0x68` to register `rax`. It then does an `xor` between the two.

`0x1a` in binary is: `11010`

And `0x68` is: `1101000`


So if we XOR these we get:

```
0011010
1101000
-------
1110010
```

With the help of pcalc we can easily translate between hex, decimal and binary.

```
# pcalc 1110010
114               0x72                0y1110010
```

And by looking at the ascii table (with `man ascii`) we see that 0x72 is equal to "r". So from there on I just kept following the flow and looking at the valuein `rax` after the `xor`. Combining them all resulted in this flag: 

```
r01l+th3m_411-up/@nd~4w@y
```


