
# 2FA broken logic

This lab's two-factor authentication is vulnerable due to its flawed logic. To solve the lab, access Carlos's account page.

Your credentials: wiener:peter
Victim's username: carlos
You also have access to the email server to receive your 2FA verification code.

Hint: Carlos will not attempt to log in to the website himself.

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/multi-factor



![img](images/2FA%20broken%20logic/1.png)

---------------------------------------------

We must use a security code sent to our email:



![img](images/2FA%20broken%20logic/2.png)


After the first login there is a redirection which creates the cookie with value “verify=wiener”:



![img](images/2FA%20broken%20logic/3.png)


We can change it to “verify=carlos”:



![img](images/2FA%20broken%20logic/4.png)


When we get to "/login2" the “verify” cookie is for carlos and we do not receive a MFA code:



![img](images/2FA%20broken%20logic/5.png)


We can send this to Intruder:



![img](images/2FA%20broken%20logic/6.png)


And we can bruteforce it with a payload like this one:



![img](images/2FA%20broken%20logic/7.png)


The code “1985” generates a different response:



![img](images/2FA%20broken%20logic/8.png)


And we log in as carlos:



![img](images/2FA%20broken%20logic/9.png)