
# Inconsistent security controls

This lab's flawed logic allows arbitrary users to access administrative functionality that should only be available to company employees. To solve the lab, access the admin panel and delete Carlos.

---------------------------------------------

References: 

- https://portswigger.net/web-security/logic-flaws/examples



![img](images/Inconsistent%20security%20controls/1.png)

---------------------------------------------


I can not register as “carlos” but yes as “test” user:



![img](images/Inconsistent%20security%20controls/2.png)


It is not possible to access “/admin”:



![img](images/Inconsistent%20security%20controls/3.png)


But you can change the email to one ending with “@dontwannacry.com”:



![img](images/Inconsistent%20security%20controls/4.png)


And then access “/admin” to delete the user:



![img](images/Inconsistent%20security%20controls/5.png)