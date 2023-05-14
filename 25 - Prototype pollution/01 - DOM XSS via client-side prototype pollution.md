
# DOM XSS via client-side prototype pollution

This lab is vulnerable to DOM XSS via client-side prototype pollution. To solve the lab:

Find a source that you can use to add arbitrary properties to the global Object.prototype.

Identify a gadget property that allows you to execute arbitrary JavaScript.

Combine these to call alert().

You can solve this lab manually in your browser, or use DOM Invader to help you.


---------------------------------------------

References: 

- https://portswigger.net/web-security/prototype-pollution/client-side



![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/1.png)

---------------------------------------------

There are 2 scripts imported in the home page:



![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/2.png)


File /resources/js/deparam.js:



![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/3.png)


File /resources/js/searchLogger.js. This one is interesting for the “transport_url” value:



![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/4.png)


We can search “/?\_\_proto\_\_[foo]=bar” and find there is a pollution:



![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/5.png)


If we add the “transport_url” element from logger.js in a request to “/?\_\_proto\_\_[transport_url]=bar”, there is a request to “/bar”:





![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/6.png)
![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/7.png)

A payload like “/?\_\_proto\_\_[transport_url]="><script>alert(1)</script><x a="” does not work:



![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/8.png)


If we search “/?\_\_proto\_\_[transport_url]=data:,alert(1);”:



![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/9.png)

------------------------------

DOM Invader is an extension in Burp's browser:











![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/10.png)
![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/11.png)
![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/12.png)
![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/13.png)
![img](images/DOM%20XSS%20via%20client-side%20prototype%20pollution/14.png)

