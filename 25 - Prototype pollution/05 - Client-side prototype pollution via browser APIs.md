
# Client-side prototype pollution via browser APIs

This lab is vulnerable to DOM XSS via client-side prototype pollution. The website's developers have noticed a potential gadget and attempted to patch it. However, you can bypass the measures they've taken.

To solve the lab:

Find a source that you can use to add arbitrary properties to the global Object.prototype.

Identify a gadget property that allows you to execute arbitrary JavaScript.

Combine these to call alert().

You can solve this lab manually in your browser, or use DOM Invader to help you.

This lab is based on real-world vulnerabilities discovered by PortSwigger Research. For more details, check out Widespread prototype pollution gadgets by Gareth Heyes.

---------------------------------------------

References: 

- https://portswigger.net/web-security/prototype-pollution/client-side/browser-apis





![img](images/Client-side%20prototype%20pollution%20via%20browser%20APIs/1.png)
![img](images/Client-side%20prototype%20pollution%20via%20browser%20APIs/2.png)
---------------------------------------------

There are 2 Javascripts scripts imported:



![img](images/Client-side%20prototype%20pollution%20via%20browser%20APIs/3.png)


File “/resources/js/searchLoggerConfigurable.js”:



![img](images/Client-side%20prototype%20pollution%20via%20browser%20APIs/4.png)


The developers used Object.defineProperty() to avoid parameter pollution. From the notes, "Developers with some knowledge of prototype pollution may attempt to block potential gadgets by using the Object.defineProperty() method. This enables you to set a non-configurable, non-writable property directly on the affected object as follows. (...) In this case, an attacker may be able to bypass this defense by polluting Object.prototype with a malicious value property. If this is inherited by the descriptor object passed to Object.defineProperty(), the attacker-controlled value may be assigned to the gadget property after all".

So instead of using “transport_url” we will use “value”:

```
/?__proto__[value]=bar
```



![img](images/Client-side%20prototype%20pollution%20via%20browser%20APIs/5.png)


And then execute an alert(1):

```
/?__proto__[value]=data:,alert(1)
```



![img](images/Client-side%20prototype%20pollution%20via%20browser%20APIs/6.png)


------------------------------





![img](images/Client-side%20prototype%20pollution%20via%20browser%20APIs/7.png)
![img](images/Client-side%20prototype%20pollution%20via%20browser%20APIs/8.png)


The request is “/?__proto__[value]=data%3A%2Calert%281%29”:



![img](images/Client-side%20prototype%20pollution%20via%20browser%20APIs/9.png)