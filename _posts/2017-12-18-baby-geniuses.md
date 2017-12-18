---
layout: post
title:  "3DSCTF2017 - Baby Geniuses"
author: "G0d3l"
tags: 3dsctf2017 web git
---

>  The babies are using their own communication method.




## Steps
- We started with a naive interaction. The website presents a login form, it does not give any feedback using all the usual HTTP verbs.
- We analysed the headers, searched for js(, css..) files but nothing unusual.
- We did a directory/file discovery on the url and it showed an exposed .git directory.
- We dumped it using a tool called [GitTools](https://github.com/internetwache/GitTools).
- We recovered a file marquee.log that contains many formatted dates.
- We tried to find a crypted message that involes the dates (practically we just guessed) but there were no interesting results except a "DKRY?T" word that we thought it's decrypt in baby language xD.
- We manually examined the .git directory  files... and we found a github repo as "origin" in the config file.
- The commit dates were actually forged. So we thought it had something to do with the contributions graph.
- The flag was actually drawn in the contributions github graph... (the commit dates).


## Git config file
{% highlight css %}
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = https://github.com/baby-geniuses-1999/babytalknetwork.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
{% endhighlight %}

## Commits log
![](https://github.com/pow270/pow270.github.io/blob/master/_posts/pictures/bg1.png?raw=true)

## Github activity
![](https://github.com/pow270/pow270.github.io/blob/master/_posts/pictures/bg2.png?raw=true)


> 3DS{gITb}
