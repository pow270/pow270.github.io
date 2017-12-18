---
layout: post
title:  "3DSCTF2017 - Scared Flag"
author: "PsycoR"
tags: 3dsctf2017 rev
---

>  Scared Flag - 477 Points
ヽ(ﾟДﾟ)ﾉ




## Steps
- We are given a hex file (which is used generally in embedded programming) compiled by arduino.
- We convert this hex file into a binary format using hex2bin command and applicate strings to it we get some fake flags.


> 3DS{by Julio Della Flora}

> 3DS{REALLY MR. FUZZER? Is that all you've got?}

 ![](https://github.com/pow270/pow270.github.io/blob/master/_posts/pictures/1.PNG?raw=true)


- After some google research we found a usefull tool to disassemble the attached hex file and extract the assembly code.
> avr-objdump -D -m avr ea137e7356e566945e51bbece00a22ad.hex > assembly.txt

- We examinate the assembly code, we found this interresting part.

 ![](https://github.com/pow270/pow270.github.io/blob/master/_posts/pictures/2.PNG?raw=true)
 
 > We convert the hex values loaded in the r24 register into string and we got the flag 3DS{youareabully}
