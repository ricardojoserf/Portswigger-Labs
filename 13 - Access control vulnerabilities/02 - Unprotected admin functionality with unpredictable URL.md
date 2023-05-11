
# Unprotected admin functionality with unpredictable URL

This lab has an unprotected admin panel. It's located at an unpredictable location, but the location is disclosed somewhere in the application.

Solve the lab by accessing the admin panel, and using it to delete the user carlos.

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/Unprotected%20admin%20functionality%20with%20unpredictable%20URL/1.png)

---------------------------------------------

By clicking Control+u we can read the source code and find the admin panel:



![img](images/Unprotected%20admin%20functionality%20with%20unpredictable%20URL/2.png)


And from the admin panel delete the user:



![img](images/Unprotected%20admin%20functionality%20with%20unpredictable%20URL/3.png)