---
layout: post
title:  "3DSCTF2017 - Wargames"
author: "PsycoR"
tags: 3dsctf2017 forensics
---

>  Joshua is making a big mess while sending some messages.




## Steps
- We are given a PBM file (portable bitmap format) which is one of the netpbm format
> PBM stores single bit pixel image as a series of ASCII "0" or "1"'s. Traditionally "0" refers to white while "1" refers to black. The header is identical to PPM and PGM format except there is no third header line (the maximum pixel value doesn't have any meaning. The magic identifier for PBM is "P1".
- We examinate the file header using an hex editor we can see the magic number P4 that doesn't seems to a PBM header at all

 ![](https://github.com/pow270/pow270.github.io/blob/master/_posts/pictures/3.PNG?raw=true)

- It looks like a PPM header
>A PPM file consists of two parts, a header and the image data. The header consists of at least three parts normally delineated by carriage returns and/or linefeeds but the PPM specification only requires white space. The first "line" is a magic PPM identifier, it can be "P3" or "P6" (not including the double quotes!). The next line consists of the width and height of the image as ASCII numbers. The last part of the header gives the maximum value of the colour components for the pixels, this allows the format to describe more than single byte (0..255) colour values. 

- We tried at the first time to change the magic number P4 to P3 but nothing special then we tried P6 and we get the correct picture that contains the flag.
> 3DS{d0nT_LaUnCH_PpMs}

 ![](https://github.com/pow270/pow270.github.io/blob/master/_posts/pictures/16eb529c67dac665dc8cfda6a185ab85.jpeg?raw=true)

