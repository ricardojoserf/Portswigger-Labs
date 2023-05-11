
# Web cache poisoning with multiple headers

This lab contains a web cache poisoning vulnerability that is only exploitable when you use multiple headers to craft a malicious request. A user visits the home page roughly once a minute. To solve this lab, poison the cache with a response that executes alert(document.cookie) in the visitor's browser.

Hint: This lab supports both the X-Forwarded-Host and X-Forwarded-Scheme headers.

---------------------------------------------

References: 

- https://portswigger.net/web-security/web-cache-poisoning

- https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws





![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/1.png)

![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/2.png)

---------------------------------------------

Generated link: https://0ae800d80308055d8643c5240076001f.web-security-academy.net/


Using Parm Miner we identify an unlinked parameter:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/3.png)

It seems there is a problem with jpg extension:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/4.png)

If we add this header to our request and send it many times, it ends up generating a 302 redirection return code:

``` 
X-Forwarded-Scheme: http
``` 



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/5.png)

If we use both headers X-Forwarded-Host and we find X-Forwarded-Scheme, we find the location becomes https://127.0.0.1/PATH:

``` 
X-Forwarded-Host: 127.0.0.1
X-Forwarded-Scheme: http
``` 



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/6.png)

If we change 127.0.0.1 for the exploit server domain name:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/7.png)

We get a redirection after some requests:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/8.png)

And the response to the redirection contains whatever we host in /bbbbbb in the exploit server:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/9.png)

There are two imported Javascript scripts in the page:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/10.png)

To solve this we must host a file in that path in the exploit server:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/11.png)

First the request will have the normal response:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/12.png)

This is the default file:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/13.png)

After hitting the cache limit there is a redirection:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/14.png)

This redirection takes to the exploit server hosted file:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/15.png)

There is an alert message if accessing the page:



![img](images/Web%20cache%20poisoning%20with%20multiple%20headers/16.png)