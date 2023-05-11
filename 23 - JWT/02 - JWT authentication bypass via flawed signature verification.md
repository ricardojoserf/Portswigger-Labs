
# JWT authentication bypass via flawed signature verification

This lab uses a JWT-based mechanism for handling sessions. The server is insecurely configured to accept unsigned JWTs.

To solve the lab, modify your session token to gain access to the admin panel at /admin, then delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

Tip: We recommend familiarizing yourself with how to work with JWTs in Burp Suite before attempting this lab.

---------------------------------------------

References: 

- https://portswigger.net/web-security/jwt



![img](images/JWT%20authentication%20bypass%20via%20flawed%20signature%20verification/1.png)

---------------------------------------------



![img](images/JWT%20authentication%20bypass%20via%20flawed%20signature%20verification/2.png)




![img](images/JWT%20authentication%20bypass%20via%20flawed%20signature%20verification/3.png)


Then delete everything after the second “.” character (that is the signature):

```
eyJraWQiOiIzNjlmMmFjZC1hZTUwLTQ4YzctYTM2Ny04NTczYzllNTc0ZmQiLCJhbGciOiJub25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3ODk4MzB9.
```



![img](images/JWT%20authentication%20bypass%20via%20flawed%20signature%20verification/4.png)


Access the admin panel and delete the user:



![img](images/JWT%20authentication%20bypass%20via%20flawed%20signature%20verification/5.png)

