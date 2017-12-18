---
layout: post
title:  "3DSCTF2017 - Bit Map"
author: "PsycoR"
tags: 3dsctf2017 forensics
---

>  We're developing our own security measures so no one will be ever able to execute our sensitive files.




## Steps
- We are given a link that contains a bunch of colored rectangles
 ![](https://github.com/pow270/pow270.github.io/blob/master/_posts/pictures/4.PNG?raw=true)

- We examine the source code we have an html table with hex colors, googling the first hex value (7F454C46) we found that is a signature of an executable linux file

 ![](https://github.com/pow270/pow270.github.io/blob/master/_posts/pictures/5.PNG?raw=true)

- We concluded that there is a hidden ELF inside colors, so we wrote a python script to extract these color values and save them in a file

{% highlight python %}
import urllib2
import binascii
from BeautifulSoup import BeautifulSoup as bs

html = urllib2.urlopen("http://bitmap01.3dsctf.org:8010/")
soup = bs(html)
bytes = ""
tds = soup.findAll('td')
for td in tds:
	bytes += binascii.a2b_hex(td['bgcolor'].replace('#',''))
file = open("binaire",'wb')
file.write(bytes)
file.close()
{% endhighlight %}

- The command file gave that its an ELF, we just need to run it and get the flag

> 3DS{H1dd3n_1n_7ru3_C0l0r5}

 ![](https://github.com/pow270/pow270.github.io/blob/master/_posts/pictures/6.png?raw=true)




