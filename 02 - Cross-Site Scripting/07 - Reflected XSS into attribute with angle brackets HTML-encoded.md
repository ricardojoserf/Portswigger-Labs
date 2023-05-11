
# Reflected XSS into attribute with angle brackets HTML-encoded

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality where angle brackets are HTML-encoded. To solve this lab, perform a cross-site scripting attack that injects an attribute and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/contexts



![img](images/Reflected%20XSS%20into%20attribute%20with%20angle%20brackets%20HTML-encoded/1.png)

---------------------------------------------

When we search “aaaa” it becomes the “value” of this HTML element:



![img](images/Reflected%20XSS%20into%20attribute%20with%20angle%20brackets%20HTML-encoded/2.png)

With this payload the alert pops:

```
" autofocus onfocus=alert(1) x="
```



![img](images/Reflected%20XSS%20into%20attribute%20with%20angle%20brackets%20HTML-encoded/3.png)


This is the HTML content which explains why the payload worked:



![img](images/Reflected%20XSS%20into%20attribute%20with%20angle%20brackets%20HTML-encoded/4.png)