
# User ID controlled by request parameter

This lab has a horizontal privilege escalation vulnerability on the user account page.

To solve the lab, obtain the API key for the user carlos and submit it as the solution.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/User%20ID%20controlled%20by%20request%20parameter/1.png)

---------------------------------------------

Clicking the “Home” button generates this GET request:



![img](images/User%20ID%20controlled%20by%20request%20parameter/2.png)


Changing the id to carlos we can get the API key:



![img](images/User%20ID%20controlled%20by%20request%20parameter/3.png)