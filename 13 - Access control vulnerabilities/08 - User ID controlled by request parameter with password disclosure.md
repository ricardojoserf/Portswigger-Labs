
# User ID controlled by request parameter with password disclosure

This lab has user account page that contains the current user's existing password, prefilled in a masked input.

To solve the lab, retrieve the administrator's password, then use it to delete carlos.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/User%20ID%20controlled%20by%20request%20parameter%20with%20password%20disclosure/1.png)

---------------------------------------------

Clicking the “Home” button generates this GET request:



![img](images/User%20ID%20controlled%20by%20request%20parameter%20with%20password%20disclosure/2.png)


We can change the id to “administrator”:



![img](images/User%20ID%20controlled%20by%20request%20parameter%20with%20password%20disclosure/3.png)


Then I changed the type to “text”:



![img](images/User%20ID%20controlled%20by%20request%20parameter%20with%20password%20disclosure/4.png)


And delete the user:



![img](images/User%20ID%20controlled%20by%20request%20parameter%20with%20password%20disclosure/5.png)