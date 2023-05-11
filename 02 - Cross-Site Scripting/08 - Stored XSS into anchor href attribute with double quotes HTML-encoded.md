
# Stored XSS into anchor href attribute with double quotes HTML-encoded

This lab contains a stored cross-site scripting vulnerability in the comment functionality. To solve this lab, submit a comment that calls the alert function when the comment author name is clicked.


---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/contexts



![img](images/Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/1.png)

---------------------------------------------

It is possible to post comments:



![img](images/Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/2.png)

This is the HTML element generated:



![img](images/Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/3.png)


When the user name is clicked, it redirects to the website set in the comment:





![img](images/Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/4.png)
![img](images/Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/5.png)

We will set the website to a javascript url:

```
javascript:alert(1)
```




![img](images/Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/6.png)


When clicked, the alert pops:



![img](images/Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/7.png)