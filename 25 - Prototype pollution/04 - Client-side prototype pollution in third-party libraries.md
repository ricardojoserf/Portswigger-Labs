
# Client-side prototype pollution in third-party libraries

This lab is vulnerable to DOM XSS via client-side prototype pollution. This is due to a gadget in a third-party library, which is easy to miss due to the minified source code. Although it's technically possible to solve this lab manually, we recommend using DOM Invader as this will save you a considerable amount of time and effort.

To solve the lab:

Use DOM Invader to identify a prototype pollution and a gadget for DOM XSS.

Use the provided exploit server to deliver a payload to the victim that calls alert(document.cookie) in their browser.

This lab is based on real-world vulnerabilities discovered by PortSwigger Research. For more details, check out Widespread prototype pollution gadgets by Gareth Heyes.


---------------------------------------------

References: 

- https://portswigger.net/web-security/prototype-pollution/client-side



![img](images/Client-side%20prototype%20pollution%20in%20third-party%20libraries/1.png)

---------------------------------------------

DOM Invader detects two sources:
 


![img](images/Client-side%20prototype%20pollution%20in%20third-party%20libraries/2.png)


In my case these two sources seem impossible to exploit.

There is a live chat where we can inject a payload in the form:



![img](images/Client-side%20prototype%20pollution%20in%20third-party%20libraries/3.png)


And DOM Invader detects a sink:



![img](images/Client-side%20prototype%20pollution%20in%20third-party%20libraries/4.png)


Finally it seems there is a gadget but my lab is not working right. The solution from Portswigger:


```
<script>
    location="https://0a65003104a4a2b382ec47a700530010.web-security-academy.net/#__proto__[hitCallback]=alert%28document.cookie%29"
</script>
```