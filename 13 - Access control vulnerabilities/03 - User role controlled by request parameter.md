
# User role controlled by request parameter

This lab has an admin panel at /admin, which identifies administrators using a forgeable cookie.

Solve the lab by accessing the admin panel and using it to delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter


---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/User%20role%20controlled%20by%20request%20parameter/1.png)

---------------------------------------------

After logging in we see a redirection where the value of the cookie “Admin” is set to “false”:



![img](images/User%20role%20controlled%20by%20request%20parameter/2.png)

Changing it to “true”, we can see and visit the admin panel to delete the user:






![img](images/User%20role%20controlled%20by%20request%20parameter/3.png)
![img](images/User%20role%20controlled%20by%20request%20parameter/4.png)