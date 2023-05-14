
# DOM XSS via an alternative prototype pollution vector

This lab is vulnerable to DOM XSS via client-side prototype pollution. To solve the lab:

Find a source that you can use to add arbitrary properties to the global Object.prototype.

Identify a gadget property that allows you to execute arbitrary JavaScript.

Combine these to call alert().

You can solve this lab manually in your browser, or use DOM Invader to help you.

Hint: Pay attention to the XSS context. You need to adjust your payload slightly to ensure that the JavaScript syntax remains valid following your injection.

---------------------------------------------

References: 

- https://portswigger.net/web-security/prototype-pollution/client-side



![img](images/DOM%20XSS%20via%20an%20alternative%20prototype%20pollution%20vector/1.png)

---------------------------------------------

There are some scripts imported when searching:



![img](images/DOM%20XSS%20via%20an%20alternative%20prototype%20pollution%20vector/2.png)


This is the file “/resources/js/searchLoggerAlternative.js”:



![img](images/DOM%20XSS%20via%20an%20alternative%20prototype%20pollution%20vector/3.png)


When accessing “/?search=aaa&\_\_proto\_\_.foo=bar”, Object.prototype has the field “foo”:



![img](images/DOM%20XSS%20via%20an%20alternative%20prototype%20pollution%20vector/4.png)


We can change it to “sequence” searching “/?search=aaa&\_\_proto\_\_.sequence=bar”. There is a problem in the eval() function stating “bar1 does not exist”:



![img](images/DOM%20XSS%20via%20an%20alternative%20prototype%20pollution%20vector/5.png)



That is because of these lines:

``` 
let a = manager.sequence || 1;
manager.sequence = a + 1;
eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');
``` 

It is necessary to add “-”, the request is "/?search=aaa&\_\_proto\_\_.sequence=alert(1)-":



![img](images/DOM%20XSS%20via%20an%20alternative%20prototype%20pollution%20vector/6.png)


-------------------------------------------------------------------





![img](images/DOM%20XSS%20via%20an%20alternative%20prototype%20pollution%20vector/7.png)
![img](images/DOM%20XSS%20via%20an%20alternative%20prototype%20pollution%20vector/8.png)
