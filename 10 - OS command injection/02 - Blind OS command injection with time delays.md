
# Blind OS command injection with time delays

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response.

To solve the lab, exploit the blind OS command injection vulnerability to cause a 10 second delay.


---------------------------------------------

References: 

- https://portswigger.net/web-security/os-command-injection



![img](images/Blind%20OS%20command%20injection%20with%20time%20delays/1.png)

---------------------------------------------

There is a function to submit feedback:



![img](images/Blind%20OS%20command%20injection%20with%20time%20delays/2.png)


The vulnerability affects the fields “Name”, “Email” and “Message”:

```
"; ping -c 127.0.0.1; echo "a
```



![img](images/Blind%20OS%20command%20injection%20with%20time%20delays/3.png)