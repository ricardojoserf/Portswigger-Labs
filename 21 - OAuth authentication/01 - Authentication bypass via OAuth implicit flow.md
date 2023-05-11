
# Authentication bypass via OAuth implicit flow

This lab uses an OAuth service to allow users to log in with their social media account. Flawed validation by the client application makes it possible for an attacker to log in to other users' accounts without knowing their password.

To solve the lab, log in to Carlos's account. His email address is carlos@carlos-montoya.net.

You can log in with your own social media account using the following credentials: wiener:peter.

---------------------------------------------

References: 

- https://portswigger.net/web-security/oauth



![img](images/Authentication%20bypass%20via%20OAuth%20implicit%20flow/1.png)

---------------------------------------------

After clicking to log in, there is a GET request:



![img](images/Authentication%20bypass%20via%20OAuth%20implicit%20flow/2.png)


Then there is a form to authenticate:



![img](images/Authentication%20bypass%20via%20OAuth%20implicit%20flow/3.png)


And then authorize the application to access some information:



![img](images/Authentication%20bypass%20via%20OAuth%20implicit%20flow/4.png)


One of the last requests is a POST request to “/authenticate” with the information of the user:



![img](images/Authentication%20bypass%20via%20OAuth%20implicit%20flow/5.png)


I will change it to the information of the user “carlos”:



![img](images/Authentication%20bypass%20via%20OAuth%20implicit%20flow/6.png)


With this we get authenticated as carlos:



![img](images/Authentication%20bypass%20via%20OAuth%20implicit%20flow/7.png)