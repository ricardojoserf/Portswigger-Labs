
# User ID controlled by request parameter with data leakage in redirect

This lab contains an access control vulnerability where sensitive information is leaked in the body of a redirect response.

To solve the lab, obtain the API key for the user carlos and submit it as the solution.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/User%20ID%20controlled%20by%20request%20parameter%20with%20data%20leakage%20in%20redirect/1.png)

---------------------------------------------

Clicking the “Home” button generates this GET request:



![img](images/User%20ID%20controlled%20by%20request%20parameter%20with%20data%20leakage%20in%20redirect/2.png)


Changing the id to “carlos” there is a redirection:



![img](images/User%20ID%20controlled%20by%20request%20parameter%20with%20data%20leakage%20in%20redirect/3.png)


In this redirect response we can find the API key:



![img](images/User%20ID%20controlled%20by%20request%20parameter%20with%20data%20leakage%20in%20redirect/4.png)