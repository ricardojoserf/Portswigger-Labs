
# User ID controlled by request parameter, with unpredictable user%20IDs

This lab has a horizontal privilege escalation vulnerability on the user account page, but identifies users with GUIDs.

To solve the lab, find the GUID for carlos, then submit his API key as the solution.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/User%20ID%20controlled%20by%20request%20parameter,%20with%20unpredictable%20user%20IDs/1.png)

---------------------------------------------

Clicking the “Home” button generates this GET request:



![img](images/User%20ID%20controlled%20by%20request%20parameter,%20with%20unpredictable%20user%20IDs/2.png)


We can get the administrator's GUID from the blog posts:



![img](images/User%20ID%20controlled%20by%20request%20parameter,%20with%20unpredictable%20user%20IDs/3.png)


And create a new request:




![img](images/User%20ID%20controlled%20by%20request%20parameter,%20with%20unpredictable%20user%20IDs/4.png)
