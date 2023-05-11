
# Username enumeration via subtly different responses

This lab is subtly vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:

- Candidate usernames

- Candidate passwords

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/password-based



![img](images/Username%20enumeration%20via%20subtly%20different%20responses/1.png)

---------------------------------------------

First a Intruder Sniper attack to enumerate the users:



![img](images/Username%20enumeration%20via%20subtly%20different%20responses/2.png)


For some usernames there is an HTML comment, and then the size of the response is higher than 3100, while for other users there is not this comment in line 14:







![img](images/Username%20enumeration%20via%20subtly%20different%20responses/3.png)
![img](images/Username%20enumeration%20via%20subtly%20different%20responses/4.png)
![img](images/Username%20enumeration%20via%20subtly%20different%20responses/5.png)


Adding a new column we find one of the responses does not contain the ending “.”:



![img](images/Username%20enumeration%20via%20subtly%20different%20responses/6.png)


We get the password when we see a redirection, so credentials are akamai:monitoring:



![img](images/Username%20enumeration%20via%20subtly%20different%20responses/7.png)

