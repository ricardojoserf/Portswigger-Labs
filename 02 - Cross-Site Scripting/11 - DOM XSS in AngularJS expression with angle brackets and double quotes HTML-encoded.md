
# DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

This lab contains a DOM-based cross-site scripting vulnerability in a AngularJS expression within the search functionality.

AngularJS is a popular JavaScript library, which scans the contents of HTML nodes containing the ng-app attribute (also known as an AngularJS directive). When a directive is added to the HTML code, you can execute JavaScript expressions within double curly braces. This technique is useful when angle brackets are being encoded.

To solve this lab, perform a cross-site scripting attack that executes an AngularJS expression and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------

There is a search function:



![img](images/DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded/1.png)


The string is part of a h1 tag:



![img](images/DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded/2.png)


Searching "<'>" we find ">", "<" and "'" are HTML-encoded:



![img](images/DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded/3.png)


Using curly-braces we find this payload is interpreted:

```
{{1== 1 ? "Yes, it is equal" : "No, it is not"}}
```





![img](images/DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded/4.png)
![img](images/DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded/5.png)


I could pop an alert with the example from https://stackoverflow.com/questions/66759842/what-does-object-constructor-constructoralert1-actually-do-in-javascript:

```
 {{constructor.constructor('alert(1)')()}}
```



![img](images/DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded/6.png)


The official solution is similar: {{$on.constructor('alert(1)')()}}