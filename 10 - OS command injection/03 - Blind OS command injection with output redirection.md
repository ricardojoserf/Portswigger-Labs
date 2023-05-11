
# Blind OS command injection with output redirection

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response. However, you can use output redirection to capture the output from the command. There is a writable folder at /var/www/images/.

The application serves the images for the product catalog from this location. You can redirect the output from the injected command to a file in this folder, and then use the image loading URL to retrieve the contents of the file.

To solve the lab, execute the whoami command and retrieve the output.

---------------------------------------------

Reference: https://portswigger.net/web-security/os-command-injection



![img](images/Blind%20OS%20command%20injection%20with%20output%20redirection/1.png)

---------------------------------------------

Generated link: https://0a19003c045b2b62809bd0800086001d.web-security-academy.net/

There is a functionality for submitting feedback:



![img](images/Blind%20OS%20command%20injection%20with%20output%20redirection/2.png)

And the images are retrieved with a GET request:



![img](images/Blind%20OS%20command%20injection%20with%20output%20redirection/3.png)

We need to execute:

```
whoami > /var/www/images/whoami.txt
```

I sent the POST request to intruder and set 3 fields to attack in Sniper mode:



![img](images/Blind%20OS%20command%20injection%20with%20output%20redirection/4.png)

Then I added 3 payloads:



![img](images/Blind%20OS%20command%20injection%20with%20output%20redirection/5.png)

When we add payloads to the field subject the website returns an error:



![img](images/Blind%20OS%20command%20injection%20with%20output%20redirection/6.png)

We get the username “peter-5fYwD0” after a GET to "/image?filename=whoami.txt", so the intruder attack worked:



![img](images/Blind%20OS%20command%20injection%20with%20output%20redirection/7.png)



![img](images/Blind%20OS%20command%20injection%20with%20output%20redirection/8.png)
