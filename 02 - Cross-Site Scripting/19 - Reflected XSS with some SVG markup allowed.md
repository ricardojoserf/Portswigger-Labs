
# Reflected XSS with some SVG markup allowed

This lab has a simple reflected XSS vulnerability. The site is blocking common tags but misses some SVG tags and events.

To solve the lab, perform a cross-site scripting attack that calls the alert() function.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts

---------------------------------------------

The content of the search is reflected inside a h1 HTML element:



![img](images/Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/1.png)


In this case it seems not even custom tags are allowed. I will test all possible tags:



![img](images/Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/2.png)


The valid tags are:
- animatetransform
- image
- title
- svg



![img](images/Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/3.png)


And then all possible attributes:
- onbegin
 


![img](images/Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/4.png)


We get this payload from https://portswigger.net/web-security/cross-site-scripting/cheat-sheet:



![img](images/Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/5.png)


```
<svg><animatetransform onbegin=alert(1) attributeName=transform>
```



![img](images/Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/6.png)
