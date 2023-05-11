
# 15 - Multi-step process with no access control on one step

This lab has an admin panel with a flawed multi-step process for changing a user's role. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.

To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator.

---------------------------------------------

Reference: https://portswigger.net/web-security/access-control

---------------------------------------------

Generated link: https://0ac700520305fa43816f2532000c0090.web-security-academy.net/


We try to send the POST request to upgrade the user:



![img](images/15%20-%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/1.png)

We get a 401 unauthorized error:



![img](images/15%20-%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/2.png)

We try with the confirmed parameter:



![img](images/15%20-%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/3.png)

We get a redirection and then an 401 unauthorized error:



![img](images/15%20-%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/4.png)

But the user was indeed promoted to administrator and the admin panel is accessible for wiener now:



![img](images/15%20-%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/5.png)