
# Authentication bypass via flawed state machine

This lab makes flawed assumptions about the sequence of events in the login process. To solve the lab, exploit this flaw to bypass the lab's authentication, access the admin interface, and delete Carlos.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/logic-flaws/examples



![img](images/Authentication%20bypass%20via%20flawed%20state%20machine/1.png)

---------------------------------------------

The requests generated when logging in:





![img](images/Authentication%20bypass%20via%20flawed%20state%20machine/2.png)
![img](images/Authentication%20bypass%20via%20flawed%20state%20machine/3.png)


Then the user selects a role:



![img](images/Authentication%20bypass%20via%20flawed%20state%20machine/4.png)


We can not access /admin:



![img](images/Authentication%20bypass%20via%20flawed%20state%20machine/5.png)


After logging in, change the “Location” header in the response to redirect to “/admin”:



![img](images/Authentication%20bypass%20via%20flawed%20state%20machine/6.png)


This changes the login to the “administrator” user and it is possible to delete the user:



![img](images/Authentication%20bypass%20via%20flawed%20state%20machine/7.png)
