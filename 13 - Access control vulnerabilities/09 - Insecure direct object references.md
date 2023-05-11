
# Insecure direct object references

This lab stores user chat logs directly on the server's file system, and retrieves them using static URLs.

Solve the lab by finding the password for the user carlos, and logging into their account.

---------------------------------------------

References: 

- https://portswigger.net/web-security/access-control



![img](images/Insecure%20direct%20object%20references/1.png)

---------------------------------------------

There is a live chat:



![img](images/Insecure%20direct%20object%20references/2.png)


It is possible to download the transcript:



![img](images/Insecure%20direct%20object%20references/3.png)


Changing to 1.txt we find the password:



![img](images/Insecure%20direct%20object%20references/4.png)