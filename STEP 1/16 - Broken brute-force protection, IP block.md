
# 16 - Broken brute-force protection, IP block

This lab is vulnerable due to a logic flaw in its password brute-force protection. To solve the lab, brute-force the victim's password, then log in and access their account page.

Your credentials: wiener:peter
Victim's username: carlos

Hint: Advanced users may want to solve this lab by using a macro or the Turbo Intruder extension. However, it is possible to solve the lab without using these advanced features.

---------------------------------------------

References:

- https://portswigger.net/web-security/authentication/password-based

- https://portswigger.net/web-security/authentication/auth-lab-passwords



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/1.png)

---------------------------------------------

Generated link: https://0a9f008704cb076382ef4c7e0060006d.web-security-academy.net/


Trying a bruteforce you get a message the IP is blocked:



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/2.png)

Attempt 8 returns incorrect password:



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/3.png)

To send the correct credentials wiener:peter after every login attempt I created a list with the password after every of the 100 passwords to test:



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/4.png)

And a list with both usernames:



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/5.png)

I wil use the Pitchfork attack type, described here, which will take the values of each list one by one:



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/6.png)

Send the payload:



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/7.png)

Now the attack is executed, but not successfully:



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/8.png)

It ends up blocked after 15 passwords, so I will add the correct login after more passwords than just one. 

I created a short Python script to generate the user list:

```
all_passw = open("100pass.txt").read().splitlines()

counter = 0
num = 5

print("wiener")
#print("peter")
for i in all_passw:
        counter += 1
        print("carlos")
        #print(i)
        if counter % num == 0:
                print("wiener")
                #print("peter")
```                
                
And the password list:

```
all_passw = open("100pass.txt").read().splitlines()

counter = 0
num = 5

#print("wiener")
print("peter")
for i in all_passw:
        counter += 1
        #print("carlos")
        print(i)
        if counter % num == 0:
                #print("wiener")
                print("peter")
```




![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/9.png)

But this did not work either.

Finally I added 1000 milliseconds between request in Resource Pool:



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/10.png)

And now it works. We find the password for carlos is “moon”:



![img](images/16%20-%20Broken%20brute-force%20protection,%20IP%20block/11.png)