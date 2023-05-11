
# Parameter cloaking

This lab is vulnerable to web cache poisoning because it excludes a certain parameter from the cache key. There is also inconsistent parameter parsing between the cache and the back-end. A user regularly visits this site's home page using Chrome.

To solve the lab, use the parameter cloaking technique to poison the cache with a response that executes alert(1) in the victim's browser.

Hint: The website excludes a certain UTM analytics parameter

---------------------------------------------

References: 

- https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws





![img](images/Parameter%20cloaking/1.png)
![img](images/Parameter%20cloaking/2.png)

---------------------------------------------

We can find the “Origin” header can work as oracle and random query parameters are keyed:

```
GET /?test=ing123 HTTP/2
...
Pragma: x-get-cache-key
Accept-Encoding: gzip, deflate, cachebuster
Accept: */*, text/cachebuster
Cookie: cachebuster=1
Origin: https://cachebuster.0afc006103e7279e80ff71c000b0005c.web-security-academy.net
```



![img](images/Parameter%20cloaking/3.png)


We will try to guess GET parameters with Param Miner:



![img](images/Parameter%20cloaking/4.png)


It detects the same parameter as the previous lab, “utm_content”, and the Parameter Cloaking problem:



![img](images/Parameter%20cloaking/5.png)




![img](images/Parameter%20cloaking/6.png)


The parameter value is reflected in the response and is not part of the “X-Cache-Key” response header:



![img](images/Parameter%20cloaking/7.png)


Hoerver we can not execute an alert(1) payload as earlier because the parameter value gets HTML-encoded:



![img](images/Parameter%20cloaking/8.png)


With a payload we see how the cookie is set to “TEST456”, so there is parameter cloaking:

```
GET /?utm_content=TEST123;utm_content=TEST456 
```



![img](images/Parameter%20cloaking/9.png)


There is a callback using the cookie to set the country:



![img](images/Parameter%20cloaking/10.png)

We can access this script with the parameter “callback” - https://0aeb0036048afd8e80b649b70032005e.web-security-academy.net/js/geolocate.js?callback=alert(1):



![img](images/Parameter%20cloaking/11.png)


We will poison the request “/js/geolocate.js?callback=setCountryCookie”, which is an endpoint requested from the home page. For that first with the payload and the “Origin” header:

```
GET /?keyed_param=abc&excluded_param=123;keyed_param=bad-stuff-here
```

```
GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert(1) HTTP/2
...
Pragma: x-get-cache-key
Origin: https://cachebuster.0afc006103e7279e80ff71c000b0005c.web-security-academy.net
```



![img](images/Parameter%20cloaking/12.png)


The keyed part was “/js/geolocate.js?callback=setCountryCookie” but the last value for “callback” was “alert(1)” so it got poisoned:



![img](images/Parameter%20cloaking/13.png)


Then accessing “/”, we do not see the alert(1) but we see the poisoned request:



![img](images/Parameter%20cloaking/14.png)


Then the request is poisoned without the “Origin” header:



![img](images/Parameter%20cloaking/15.png)


And then access “/”:





![img](images/Parameter%20cloaking/16.png)
![img](images/Parameter%20cloaking/17.png)