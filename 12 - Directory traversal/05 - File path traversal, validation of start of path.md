
# File path traversal, validation of start of path

This lab contains a file path traversal vulnerability in the display of product images.

The application transmits the full file path via a request parameter, and validates that the supplied path starts with the expected folder.

To solve the lab, retrieve the contents of the /etc/passwd file.

---------------------------------------------

References: 

- https://portswigger.net/web-security/file-path-traversal




![img](images/File%20path%20traversal,%20validation%20of%20start%20of%20path/1.png)

---------------------------------------------

To retrieve an image the application uses a GET request with the parameter filename and the full path:



![img](images/File%20path%20traversal,%20validation%20of%20start%20of%20path/2.png)


To retrieve /etc/passwd we need the path to start with “/var/www/images/”:

```
GET /image?filename=/var/www/images/../../../etc/passwd 
```



![img](images/File%20path%20traversal,%20validation%20of%20start%20of%20path/3.png)
