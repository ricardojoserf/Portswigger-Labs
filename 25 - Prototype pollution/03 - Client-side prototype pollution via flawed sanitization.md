
# Client-side prototype pollution via flawed sanitization

This lab is vulnerable to DOM XSS via client-side prototype pollution. Although the developers have implemented measures to prevent prototype pollution, these can be easily bypassed.

To solve the lab:

Find a source that you can use to add arbitrary properties to the global Object.prototype.

Identify a gadget property that allows you to execute arbitrary JavaScript.

Combine these to call alert().


---------------------------------------------

References: 

- https://portswigger.net/web-security/prototype-pollution/client-side



![img](images/Client-side%20prototype%20pollution%20via%20flawed%20sanitization/1.png)

---------------------------------------------

There are two scripts imported in the home page:



![img](images/Client-side%20prototype%20pollution%20via%20flawed%20sanitization/2.png)



This is “/resources/js/searchLoggerFiltered.js”. It suffers the same problem that one previous lab with “transport_url”:



![img](images/Client-side%20prototype%20pollution%20via%20flawed%20sanitization/3.png)


Knowing the sanitization from the santizeKey() function, we can try a payload like:

```
/?__pro__proto__to__[transport_url]=bar
```

And the value in Object.prototype is correct:



![img](images/Client-side%20prototype%20pollution%20via%20flawed%20sanitization/4.png)


Finally:

```
/?__pro__proto__to__[transport_url]=data:,alert(1);
```



![img](images/Client-side%20prototype%20pollution%20via%20flawed%20sanitization/5.png)