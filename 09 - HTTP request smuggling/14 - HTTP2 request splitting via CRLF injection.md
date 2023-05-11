
# HTTP/2 request splitting via CRLF injection

This lab is vulnerable to request smuggling because the front-end server downgrades HTTP/2 requests and fails to adequately sanitize incoming headers.

To solve the lab, delete the user carlos by using response queue poisoning to break into the admin panel at /admin. An admin user will log in approximately every 10 seconds.

The connection to the back-end is reset every 10 requests, so don't worry if you get it into a bad state - just send a few normal requests to get a fresh connection.

Hint: To inject newlines into HTTP/2 headers, use the Inspector to drill down into the header, then press the Shift + Return keys. Note that this feature is not available when you double-click on the header.

---------------------------------------------

References: 

- https://portswigger.net/web-security/request-smuggling/advanced



![img](images/HTTP2%20request%20splitting%20via%20CRLF%20injection/1.png)

---------------------------------------------

Intercept the GET request and add “\r\n”, the “Host” header, “\r\n”, “\r\n” and the new request:



![img](images/HTTP2%20request%20splitting%20via%20CRLF%20injection/2.png)


Every two requests, we get a request to “/admin”, but no access to it:



![img](images/HTTP2%20request%20splitting%20via%20CRLF%20injection/3.png)


Then I added the “Host” header to the second request too:

```
ba
Host: 0a5f00f003d0fe7d8643783800450004.web-security-academy.net

GET /admin HTTP/1.1
Host: 0a5f00f003d0fe7d8643783800450004.web-security-academy.net
```

The response queue is poisoned after this. I executed a GET request to “/” every second, so the first request after the smuggling returns a 401 error code, the response from the access to “/admin”, and the following requests may get the response from the victim user. In this case I got a 302 redirection code, with the cookie of the victim: 



![img](images/HTTP2%20request%20splitting%20via%20CRLF%20injection/4.png)


With this cookie it is possible to access the “/admin” panel:



![img](images/HTTP2%20request%20splitting%20via%20CRLF%20injection/5.png)


And then delete the user:



![img](images/HTTP2%20request%20splitting%20via%20CRLF%20injection/6.png)