
# Password brute-force via password change

This lab's password change functionality makes it vulnerable to brute-force attacks. To solve the lab, use the list of candidate passwords to brute-force Carlos's account and access his "My account" page.

- Your credentials: wiener:peter

- Victim's username: carlos

- Candidate passwords

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/other-mechanisms



![img](images/Password%20brute-force%20via%20password%20change/1.png)

---------------------------------------------

There is a function to update the password:



![img](images/Password%20brute-force%20via%20password%20change/2.png)


It generates a POST request with the username and current password as parameters:



![img](images/Password%20brute-force%20via%20password%20change/3.png)


When the current password is correct but the new passwords are different the message is:



![img](images/Password%20brute-force%20via%20password%20change/4.png)



When the new passwords do not match and the current password is wrong the message is:



![img](images/Password%20brute-force%20via%20password%20change/5.png)


We can change the username and send it to Intruder to test all the passwords:



![img](images/Password%20brute-force%20via%20password%20change/6.png)


From this we get that the password is “12345”:



![img](images/Password%20brute-force%20via%20password%20change/7.png)
