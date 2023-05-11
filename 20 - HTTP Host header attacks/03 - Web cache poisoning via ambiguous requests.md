
# Web cache poisoning via ambiguous requests

This lab is vulnerable to web cache poisoning due to discrepancies in how the cache and the back-end application handle ambiguous requests. An unsuspecting user regularly visits the site's home page.

To solve the lab, poison the cache so the home page executes alert(document.cookie) in the victim's browser.

---------------------------------------------

References: 

- https://portswigger.net/web-security/host-header/exploiting



![img](images/Web%20cache%20poisoning%20via%20ambiguous%20requests/1.png)

---------------------------------------------

We can not find the cache keys using “Pragma: x-get-cache-key” but we can read from the response headers that the cache lasts 30 seconds. 

The “Host” header is reflected but if you use a random one there is a gateway error, so it is necessary to use 2, the first one, malicious, is reflected and the second is the legitimate one:



![img](images/Web%20cache%20poisoning%20via%20ambiguous%20requests/2.png)


We can execute Javascript code with a payload like this:

```
GET / HTTP/2
Host: testing123"></script><script>alert(document.cookie)</script><script>
Host: 0af500ba04a9e93b802c499200750009.web-security-academy.net
...
```



![img](images/Web%20cache%20poisoning%20via%20ambiguous%20requests/3.png)


And we see the alert with the cookie:



![img](images/Web%20cache%20poisoning%20via%20ambiguous%20requests/4.png)