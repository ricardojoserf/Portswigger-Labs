
# Stored DOM XSS

This lab demonstrates a stored DOM vulnerability in the blog comment functionality. To solve this lab, exploit this vulnerability to call the alert() function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------

It is possible to post comments:



![img](images/Stored%20DOM%20XSS/1.png)


It generates the following HTML code:



![img](images/Stored%20DOM%20XSS/2.png)

We can try the payload:

```
</p><img src=x onerror=alert(1) /><p>
```



![img](images/Stored%20DOM%20XSS/3.png)


It pops an alert:



![img](images/Stored%20DOM%20XSS/4.png)