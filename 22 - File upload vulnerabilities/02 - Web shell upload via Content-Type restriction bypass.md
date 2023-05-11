
# Web shell upload via Content-Type restriction bypass

This lab contains a vulnerable image upload function. It attempts to prevent users from uploading unexpected file types, but relies on checking user-controllable input to verify this.

To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file /home/carlos/secret. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/file-upload



![img](images/Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/1.png)

---------------------------------------------

If we try the same payload as the previous lab there is an error:



![img](images/Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/2.png)


Changing the Content-Type it is possible to upload the PHP file:



![img](images/Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/3.png)


And read the secret ("wDHZLacPXl2c4B4MZl2j7T3MluCqDzjR"):



![img](images/Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/4.png)