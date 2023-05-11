
# Stored XSS into HTML context with nothing encoded

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed.

---------------------------------------------

Reference: https://portswigger.net/web-security/cross-site-scripting/stored

---------------------------------------------

There is a functionality to post comments in each blog post:



![img](images/Stored%20XSS%20into%20HTML%20context%20with%20nothing%20encoded/1.png)


If you check the blog post again you see the alert popping:



![img](images/Stored%20XSS%20into%20HTML%20context%20with%20nothing%20encoded/2.png)
