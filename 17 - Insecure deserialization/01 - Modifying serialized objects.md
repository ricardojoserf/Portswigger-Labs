
# Modifying serialized objects

This lab uses a serialization-based session mechanism and is vulnerable to privilege escalation as a result. To solve the lab, edit the serialized object in the session cookie to exploit this vulnerability and gain administrative privileges. Then, delete Carlos's account.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/deserialization/exploiting



![img](images/Modifying%20serialized%20objects/1.png)

---------------------------------------------

After logging in, there is a cookie with value “Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjowO30%3d”:



![img](images/Modifying%20serialized%20objects/2.png)


We can decode it in the Decoder:



![img](images/Modifying%20serialized%20objects/3.png)

We can change the value to 1 and encode:



![img](images/Modifying%20serialized%20objects/4.png)


With this cookie we can see the admin panel:





![img](images/Modifying%20serialized%20objects/5.png)
![img](images/Modifying%20serialized%20objects/6.png)
