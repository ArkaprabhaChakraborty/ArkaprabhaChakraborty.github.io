+++
showdate = true
title = "HTB Web Challenge walkthrough with OWASP ZAP: Templated"
date = 2022-10-04
url = "/blog/templated_htb_web_challenge_zap_walkthrough"
+++

So I decided to take the [Hack The Box(HTB)](https://www.hackthebox.com/) Web Challenges with [OWASP ZAP](https://www.zaproxy.org/). This blog is a walkthrough of the "Templated" web challenge in HTB, shout out to [clubby789](https://app.hackthebox.com/users/83743) for creating this challenge.

# What is ZAP?
Zed Attack Proxy (ZAP) is the world's most popular open source web application scanner. ZAP is free to use and tons of different add-ons are available to extend its functionality. Even though ZAP is officially called a "web application scanner", we, the contributors and the core team of ZAP, sometimes unofficialy call it an "Integrated Hacking Environment (IHE)".

You can install ZAP from [here](https://www.zaproxy.org/download/). Or you can also install a "dev" build by following the instrcutions [here](https://www.zaproxy.org/docs/developer/quick-start-build/). I would recommend you to install the "dev" build as it has the latest unreleased features :P.

# Recon process
After installaing ZAP, head to the manual explore option in the quick start panel in ZAP's workspace window (More about ZAP's GUI [here](https://www.zaproxy.org/getting-started/#zap-desktop-ui)). I'm enabling HUD, but you can skip that option if you want to. Copy the docker instance's socket address and paste it in the "URL to explore" field. Then click on "Launch browser" button. This will open up a new tab in your browser. You can use this tab to explore the target. I'm using Firefox for this walkthrough.

![ZAP GUI image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/manual%20explore.png?raw=true)

After launching the browser we get th following screen:

![Templated Web Challenge Initial Screen](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/starting%20page.png?raw=true)

"Powered by Flask/Jinja2"... Huh!... So, initially, the site has nothing much to show apart from this "statement". Since the challenge name is "Templated" and it's an EASY challenge, it's template injection for sure :P. Let's discover more about the site.

In the response I find this `Server: Werkzeug/1.0.1 Python/3.9.0`. Flask is a python web framework and Werkzueg (I have no idea how to pronounce this) is a Web Server Gateway Interface (WSGI) library. WSGI is a specification that describes how a web server communicates with web applications, and how web applications can be chained together to process one request (More about WSGI [here](https://wsgi.readthedocs.io/en/latest/index.html)). Now Werkzeug has a debug console, which can be accessed at `/console`. However this has a [debugger pin](https://werkzeug.palletsprojects.com/en/2.2.x/debug/#debugger-pin) by default. 

On visiting https://<socket>/console I find this:
![Error 404 Image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/error%20page.png?raw=true)
So there is no debug console, ie, debugger is not enabled, but, the 'console' was reflected. Just to be sure that it's templated, I tried a new random endpoint `/test` and it was reflected too. So, it's templated for sure :P.

# A short background on SSTI
SSTI or Server Side Template Injection is a technique in which attacker injects malicious code into a server using the native template syntax. Read more: 
- [OWASP SSTI](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server_Side_Template_Injection)
- [Portswigger labs](https://portswigger.net/web-security/server-side-template-injection)
- [James Kettle's whitepaper](https://portswigger.net/kb/papers/serversidetemplateinjection.pdf)
- [Hacktricks](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)


# SSTI with Jinja2

Werkzeug uses jinja2 templates and flask uses werkzeug. So I did what everyone does, I googled for "flask jinja2 ssti". Found this wonderful [blog](https://www.lanmaster53.com/2016/03/exploring-ssti-flask-jinja2/) from [Timothy Tomes](https://www.linkedin.com/in/lanmaster53/). 

So I tried the following the browser address bar `https://<socket_addr>/console{{7*7}}` and returned `console49` could not be found. Great!

Time to sniff out secrets and possibly find out the flag. Tried `https://<socket_addr>/console{{config}}` and got this:
![Config Image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/config%20reflection.png?raw=true)
Welp!! ... found everything other than the flag.

Let's try out something more with the config object using ZAP's Request Editor. This can be done easily by right clicking on the request in ZAP's workspace and selecting "Open/Resend with Request Editor". I tried modifying the URL in request header, appending the following template to try and get an injection:
```python3
{{config.from_object("os")}}
``` 
but returned it `None` :). SWEET! :) 

We can get to the *os* library using something/concept called *Method Resolution Order (MRO)*. (More about MRO [here](https://levelup.gitconnected.com/method-resolution-order-in-python-5afbaecc25e0)).
Now let's discover the MRO of the config object. I tried: 
```python3
{{config.__class__.mro()}}
```
hit send and got this:

![URL encoded config image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/config%20mro.png?raw=true)

The encoded output can be a pain in the eyes so you can select the response body, right click in the reposne body and click on the "Encode/Decode/Hash" option. In Decode panel under "HTML Decode" you can see the famailar python3 output.

![URL Decoded image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/decode.png?raw=true)

From this we can see conclude that we cannot access the os module from here as there is no reference to *\_\_globals\_\_* :) . Without the *\_\_globals\_\_* reference we cannot access *\_\_builtins\_\_* and without *\_\_builtins\_\_* we cannot access the os module. :)

So time to try something else. Let's try and see if the site has been configured/ written like the flask bolier plates using an *app* object. I tried `{{app.__class__.mro()}}` and got a 500 error. :/

Let's try this now 
```python3
{{app.__class__.__init__.__globals__}}
``` 
It works! We are in business!! :D

![App globals image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/global.png?raw=true)
We can now use *\_\_builtins\_\_* to access the os module. Let's try 
```python3
{{app.__class__.__init__.__globals__.__builtins__}}
``` 
and gotcha:

![App builtins image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/builtins.png?raw=true)

Now let's import the os module and get the user ids. I tried 
```python3
{{app.__class__.__init__.__globals__.__builtins__.__import__("os").getuid()}}
``` 
and :)

![UserId image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/userid.png?raw=true)

# Time to get the flag

Well we are "in". So now time to discover the server. I tried 
```python3
{{app.__class__.__init__.__globals__.__builtins__.__import__("os").popen("ls").read()}}
```
and got this:

![ls image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/ls%20result.png?raw=true)

So we have a *flag.txt* file. Let's try to read it. I tried
```python3
{{app.__class__.__init__.__globals__.__builtins__.__import__("os").popen("cat flag.txt").read()}}
``` 
and got this:

![Flag image](https://github.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/blob/main/data/images/templated%20flag.png?raw=true)

There is our 31337 string. :)

# Conclusion

This was a fun challenge. SSTI's like these can be stopped if the input is sanitized and validated properly. That being said it was a very beginner friendly challenge. I hope you enjoyed reading this writeup/blog. :)
