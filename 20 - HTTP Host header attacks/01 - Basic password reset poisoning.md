
# Basic password reset poisoning

This lab is vulnerable to password reset poisoning. The user carlos will carelessly click on any links in emails that he receives. To solve the lab, log in to Carlos's account.

You can log in to your own account using the following credentials: wiener:peter. Any emails sent to this account can be read via the email client on the exploit server.

---------------------------------------------

References: 

- https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning



![img](images/Basic%20password%20reset%20poisoning/1.png)

---------------------------------------------

There is a function to restore forgotten passwords:



![img](images/Basic%20password%20reset%20poisoning/2.png)


It generates a POST request for the user “wiener”:



![img](images/Basic%20password%20reset%20poisoning/3.png)


We can change the “Host” header for the POST request to “/forgot-password” and the “username” parameter to the value “carlos”:



![img](images/Basic%20password%20reset%20poisoning/4.png)


And there is a request to the exploit server:



![img](images/Basic%20password%20reset%20poisoning/5.png)


Using this token we can change the password:



![img](images/Basic%20password%20reset%20poisoning/6.png)


And access the page as carlos:



![img](images/Basic%20password%20reset%20poisoning/7.png)