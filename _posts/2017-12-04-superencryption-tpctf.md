---
layout: post
title:  "TPCTF2017 - superencrypt"
author: "Rettila"
tags: tpctf2017 rev 
---

> My friend sent me a flag encrypted with an encryption program. Unfortunately, the decryption doesn't seem to work. Please help me decrypt this: dufhyuc\>bi\{\{f0\|;vwh\<\~b5p5thjq6goj\}  
Author: Kevin Higgs

## Analyzing the program
- By trying some inputs and watching the given outputs, we conclude that the output's length depends on the input's length.
- Our encrypted string has a length of 34 characters, we can easily find that the input must be a 34 character string too.
- By giving some inputs and modifying only one character, we can see that only one character changes in the output. 
- We also notice that the encryted letter is just shifted.

## Writing the decryption program
{% highlight python %}
from pwn import *
import string
import time

#Calulacting the decrypted character ( ord(a)-ord(b) ) is the decryption key
def decode(a,b,c):	
	return chr(ord(c)+ord(a)-ord(b)) 

#Dismatch function finds the new position of the modified character
def dismatch(str1,str2):	
    for i in range(0,len(str1)):
        if (str1[i] != str2[i] ):
            return i
    return -1
	
context(arch = 'amd64', os = 'linux', endian = 'little', word_size = 32)
binary = './superencrypt'  
{% raw %}enc_flag = "dufhyuc>bi{{f0|;vwh<~b5p5thjq6goj}"{% endraw %}
flag = list("-"*34)
for offset in range(0,34):
    rev = process(binary,stdin=process.PTY)
    rev.sendlineafter("Please enter 0 to encrypt or 1 to decrypt:","0")
    rev.sendlineafter("Enter the string you want to encrypt: ", ''.join(flag))
    encrypted_flag1 = rev.recvline()
    rev.close()
    #Just modifying one character of the input
    flag[offset] = '/'	
    rev = process(binary,stdin=process.PTY)
    rev.sendlineafter("Please enter 0 to encrypt or 1 to decrypt:","0")
    rev.sendlineafter("Enter the string you want to encrypt: ", ''.join(flag))
    encrypted_flag2 = rev.recvline()
    rev.close()
    image = dismatch(encrypted_flag1,encrypted_flag2)
    flag[offset] = decode(flag[offset] ,encrypted_flag2[image] ,enc_flag[image])
    print ''.join(flag)
{% endhighlight %}

The flag is: tpctf\{Y4Y_f0r_r3v3rse_3ngin33ring\}
