
# User role can be modified in user profile

This lab has an admin panel at /admin. It's only accessible to logged-in users with a roleid of 2.

Solve the lab by accessing the admin panel and using it to delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/User%20role%20can%20be%20modified%20in%20user%20profile/1.png)

---------------------------------------------

Login process is a POST request:



![img](images/User%20role%20can%20be%20modified%20in%20user%20profile/2.png)


It responds with a 302 redirect:



![img](images/User%20role%20can%20be%20modified%20in%20user%20profile/3.png)


There is a function to update the email address with a POST request:



![img](images/User%20role%20can%20be%20modified%20in%20user%20profile/4.png)


It responds with the personal information:



![img](images/User%20role%20can%20be%20modified%20in%20user%20profile/5.png)


We can add the roleid parameter to the POST request to change the role of the user:

```
POST /my-account/change-email HTTP/2
...

{"email":"test@test.com", "roleid":2}
```



![img](images/User%20role%20can%20be%20modified%20in%20user%20profile/6.png)


It is updated:



![img](images/User%20role%20can%20be%20modified%20in%20user%20profile/7.png)


Then we can delete the user:



![img](images/User%20role%20can%20be%20modified%20in%20user%20profile/8.png)