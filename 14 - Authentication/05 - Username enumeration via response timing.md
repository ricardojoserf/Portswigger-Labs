
# Username enumeration via response timing

This lab is vulnerable to username enumeration using its response times. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

Your credentials: wiener:peter

- Candidate usernames

- Candidate passwords

Hint: To add to the challenge, the lab also implements a form of IP-based brute-force protection. However, this can be easily bypassed by manipulating HTTP request headers.


---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/password-based



![img](images/Username%20enumeration%20via%20response%20timing/1.png)

---------------------------------------------

I will set a Pitchfork attack adding the “X-Forwarded-For” header:



![img](images/Username%20enumeration%20via%20response%20timing/2.png)


I will add numbers to the IP address from that header:



![img](images/Username%20enumeration%20via%20response%20timing/3.png)


There is a button “Columns” at the top:



![img](images/Username%20enumeration%20via%20response%20timing/4.png)


We can see which requests took longer. For this, it is important to set a very long password:



![img](images/Username%20enumeration%20via%20response%20timing/5.png)


There is a redirection so credentials are accounts:jessica:



![img](images/Username%20enumeration%20via%20response%20timing/6.png)