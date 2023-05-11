
# Authentication bypass via encryption oracle

This lab contains a logic flaw that exposes an encryption oracle to users. To solve the lab, exploit this flaw to gain access to the admin panel and delete Carlos.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/logic-flaws/examples



![img](images/Authentication%20bypass%20via%20encryption%20oracle/1.png)

---------------------------------------------

After logging in, there is a “stay-logged-in” cookie:



![img](images/Authentication%20bypass%20via%20encryption%20oracle/2.png)


The value seems encrypted:



![img](images/Authentication%20bypass%20via%20encryption%20oracle/3.png)


When a comment is sent there is a cookie “notification” with an encrypted value:



![img](images/Authentication%20bypass%20via%20encryption%20oracle/4.png)


This notification is probably the message that appears in the post, in this case “Invalid email address: test”:



![img](images/Authentication%20bypass%20via%20encryption%20oracle/5.png)


With a value “a” in this cookie we generate an internal server error:



![img](images/Authentication%20bypass%20via%20encryption%20oracle/6.png)


With a base64-encoded value “YQ==” we see information about the encryption. It looks it uses padded cipher and blocks of 16 bytes, so it could be AES-128:



![img](images/Authentication%20bypass%20via%20encryption%20oracle/7.png)


We can decrypt the "stay-logged-in" cookie value, it is “wiener:1683621955878”. The second value is the EPOCH time when I logged in earlier.



![img](images/Authentication%20bypass%20via%20encryption%20oracle/8.png)


Knowing it uses 16-bytes blocks and there is a prefix of 23 characters ("Invalid email address: “), we will add 9 characters of ”padding" in that second ciphered block:



![img](images/Authentication%20bypass%20via%20encryption%20oracle/9.png)


We will take the encrypted value, URL-decode and base64-decode it, and delete the first 2 16-bytes blocks:

```
bRmw%2bImnDFvvwECQSiG1J8dfoUcrqKgkyQfr8wfI1J9bRCG%2bSLS06HPtXsMPhzuBQTkxD8oSxM2l3LhRCdZ3IQ%3d%3d
bRmw+ImnDFvvwECQSiG1J8dfoUcrqKgkyQfr8wfI1J9bRCG+SLS06HPtXsMPhzuBQTkxD8oSxM2l3LhRCdZ3IQ==
...
W0Qhvki0tOhz7V7DD4c7gUE5MQ/KEsTNpdy4UQnWdyE=
W0Qhvki0tOhz7V7DD4c7gUE5MQ/KEsTNpdy4UQnWdyE%3d
```



![img](images/Authentication%20bypass%20via%20encryption%20oracle/10.png)


First we will set the value “W0Qhvki0tOhz7V7DD4c7gUE5MQ/KEsTNpdy4UQnWdyE%3d” for the “notification” cookie to check it is decrypted correctly:



![img](images/Authentication%20bypass%20via%20encryption%20oracle/11.png)


We can delete the "session" cookie and use this value to log in:

```
GET /my-account?id=administrator HTTP/2
...
Cookie: stay-logged-in=W0Qhvki0tOhz7V7DD4c7gUE5MQ%2fKEsTNpdy4UQnWdyE%3d
...
```



![img](images/Authentication%20bypass%20via%20encryption%20oracle/12.png)


And then delete the user:



![img](images/Authentication%20bypass%20via%20encryption%20oracle/13.png)

