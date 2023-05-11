
# Referer-based access control

This lab controls access to certain admin functionality based on the Referer header. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.

To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator.

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/Referer-based%20access%20control/1.png)

---------------------------------------------

The process to upgrade the user is generated from the admin panel with a GET request:



![img](images/Referer-based%20access%20control/2.png)


But with “wiener” we get an unauthorized error:



![img](images/Referer-based%20access%20control/3.png)


It works chaning the “Referer” header to /admin:

```
GET /admin-roles?username=wiener&action=upgrade HTTP/2
...
Referer: https://0afd00fc039e68c881769dfa009b00b8.web-security-academy.net/admin
...
```



![img](images/Referer-based%20access%20control/4.png)