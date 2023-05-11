
# 2FA simple bypass

This lab's two-factor authentication can be bypassed. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, access Carlos's account page.

- Your credentials: wiener:peter

- Victim's credentials carlos:montoya

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/multi-factor



![img](images/2FA%20simple%20bypass/1.png)

---------------------------------------------

After authenticating we find there are 2 pages: “/” and “/my-account?id=wiener”:



![img](images/2FA%20simple%20bypass/2.png)


After logging in as carlos we have to send the security code:



![img](images/2FA%20simple%20bypass/3.png)


We can access / and then click “Home”:




![img](images/2FA%20simple%20bypass/4.png)