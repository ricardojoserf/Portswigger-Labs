
# Reflected XSS into a JavaScript string with angle brackets HTML encoded

This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality where angle brackets are encoded. The reflection occurs inside a JavaScript string. To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/contexts




![img](images/Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20HTML%20encoded/1.png)

---------------------------------------------


When we search “aaaa”, it generates a page with the following code:



![img](images/Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20HTML%20encoded/2.png)

With a payload like:

```
';alert(1);echo 'a
```

We see the code is now:



![img](images/Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20HTML%20encoded/3.png)

With this payload the alert pops:

```
';alert(1)//
```



![img](images/Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20HTML%20encoded/4.png)