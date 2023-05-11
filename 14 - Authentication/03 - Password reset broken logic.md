
# Password reset broken logic

This lab's password reset functionality is vulnerable. To solve the lab, reset Carlos's password then log in and access his "My account" page.

- Your credentials: wiener:peter

- Victim's username: carlos

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/other-mechanisms




![img](images/Password%20reset%20broken%20logic/1.png)

---------------------------------------------

There is a “Forgot password” link in the login page:



![img](images/Password%20reset%20broken%20logic/2.png)


We receive a message with a link to restore the password:



![img](images/Password%20reset%20broken%20logic/3.png)


There we submit the new password:



![img](images/Password%20reset%20broken%20logic/4.png)


It generates the POST request:



![img](images/Password%20reset%20broken%20logic/5.png)


So we can change the username parameter to “carlos” and access the page with the new password:

```
POST /forgot-password?temp-forgot-password-token=eeiyUnDFWhZqlaBJz707cipGPxh7bN4T HTTP/2
...

temp-forgot-password-token=eeiyUnDFWhZqlaBJz707cipGPxh7bN4T&username=carlos&new-password-1=password&new-password-2=password
```