
# Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality. The reflection occurs inside a template string with angle brackets, single, and double quotes HTML encoded, and backticks escaped. To solve this lab, perform a cross-site scripting attack that calls the alert function inside the template string.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts



![img](images/Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets,%20single,%20double%20quotes,%20backslash%20and%20backticks/1.png)

---------------------------------------------

The content of the search is reflected inside the variable “message”, a template literal:



![img](images/Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets,%20single,%20double%20quotes,%20backslash%20and%20backticks/2.png)


We can execute this payload inside the template literal:

```
${alert(1)}
```



![img](images/Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets,%20single,%20double%20quotes,%20backslash%20and%20backticks/3.png)