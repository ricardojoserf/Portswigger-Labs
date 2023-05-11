
# JWT authentication bypass via unverified signature

This lab uses a JWT-based mechanism for handling sessions. Due to implementation flaws, the server doesn't verify the signature of any JWTs that it receives.

To solve the lab, modify your session token to gain access to the admin panel at /admin, then delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

Tip: We recommend familiarizing yourself with how to work with JWTs in Burp Suite before attempting this lab.

---------------------------------------------

References: 

- https://portswigger.net/web-security/jwt



![img](images/JWT%20authentication%20bypass%20via%20unverified%20signature/1.png)

---------------------------------------------

After logging in we are assigned a JWT:



![img](images/JWT%20authentication%20bypass%20via%20unverified%20signature/2.png)


We can change this JWT in the JSON Web Token extension panel:



![img](images/JWT%20authentication%20bypass%20via%20unverified%20signature/3.png)


Getting the JWT:

```
eyJraWQiOiI0MTZkMDg2Yy00MDdhLTRiYzQtODhhMy00MzAyZTUzMTk1ZTgiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3ODg2MzR9.qmaz_uqHRR06JTx5vtenCveTPOtzi3mG0X1WMJhnKV3AmzlI3Pjceo3Lldu2oLHcP9SEblyJxJJ5hIO3VVAKzWsWGjNw4aN1vZCBhxzcY-MgxuspBc3XpS1_oMeenFcfEn0I4Jlob_YMrZVqQbdp8i1w_SpYLkMOkDaLlgPZk3TwZa1U005YBhHjQrItMBYWRtQDnP4rYnHkTsgwmWRu8RMCirq9-SS9gczbr2YEENZuPrxWphbYwCSMtivcysFOKXEzCvO7juIKqAfE_WmB6qx41I8Wny-qlkbeU3-9VXyIM8iC6opD6wlUiI9S328bjXN_ZFWsuRdaDVyvE4gRXw
```

Then I changed the “Location” header to “/admin”:



![img](images/JWT%20authentication%20bypass%20via%20unverified%20signature/4.png)


Getting access to the admin panel:



![img](images/JWT%20authentication%20bypass%20via%20unverified%20signature/5.png)
