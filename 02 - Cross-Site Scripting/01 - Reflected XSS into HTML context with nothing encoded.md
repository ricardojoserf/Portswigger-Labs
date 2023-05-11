
# Reflected XSS into HTML context with nothing encoded

This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

To solve the lab, perform a cross-site scripting attack that calls the alert function.

---------------------------------------------

Reference: https://portswigger.net/web-security/cross-site-scripting/reflected

---------------------------------------------

There is a search functionality that takes the user input and uses it to generate the next HTML code:





![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded/1.png)
![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded/2.png)


Searching “<script>alert(1)</script>” you see the alert popping:





![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded/3.png)
![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20nothing%20encoded/4.png)
