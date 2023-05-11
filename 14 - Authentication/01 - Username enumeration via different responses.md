
# Username enumeration via different responses

This lab is vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:

- Candidate usernames

- Candidate passwords

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/password-based



![img](images/Username%20enumeration%20via%20different%20responses/1.png)

---------------------------------------------

I send the POST request from the login to Intruder and set the attack type to “Pitchfork” (this was a mistake, to test all usernames and passwords it should be “Cluster bomb”, but it is faster using “Sniper” or “Pitchfork”):



![img](images/Username%20enumeration%20via%20different%20responses/2.png)


Then paste the usernames in the set 1 and passwords in set 2:



![img](images/Username%20enumeration%20via%20different%20responses/3.png)


From the response size we can detect a different response using username “apollo”:



![img](images/Username%20enumeration%20via%20different%20responses/4.png)


With a normal attack setting the username to “apollo” and testing the 100 passwords we get the credentials apollo:batman:



![img](images/Username%20enumeration%20via%20different%20responses/5.png)