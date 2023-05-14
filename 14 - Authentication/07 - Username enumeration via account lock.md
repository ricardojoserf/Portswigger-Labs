
# Username enumeration via account lock

This lab is vulnerable to username enumeration. It uses account locking, but this contains a logic flaw. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

- Candidate usernames

- Candidate passwords

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/password-based



![img](images/Username%20enumeration%20via%20account%20lock/1.png)

---------------------------------------------

Intercept the login POST request and send it to Intruder:



![img](images/Username%20enumeration%20via%20account%20lock/2.png)


The user “pi” was locked:



![img](images/Username%20enumeration%20via%20account%20lock/3.png)


Then we will set the username and test all passwords:



![img](images/Username%20enumeration%20via%20account%20lock/4.png)


One request does not return the message of invalid password or too many requests, it seems it is blank. This is the correct password:




![img](images/Username%20enumeration%20via%20account%20lock/5.png)