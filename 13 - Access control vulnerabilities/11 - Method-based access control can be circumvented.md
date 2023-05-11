
# Method-based access control can be circumvented

This lab implements access controls based partly on the HTTP method of requests. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.

To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator.

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/Method-based%20access%20control%20can%20be%20circumvented/1.png)

---------------------------------------------

Admin panel has functionality to upgrade users:



![img](images/Method-based%20access%20control%20can%20be%20circumvented/2.png)


This is a POST request:



![img](images/Method-based%20access%20control%20can%20be%20circumvented/3.png)


But it also works if it is a GET request:



![img](images/Method-based%20access%20control%20can%20be%20circumvented/4.png)


If we execute this as wiener we can upgrade ourselves to administrators:



![img](images/Method-based%20access%20control%20can%20be%20circumvented/5.png)