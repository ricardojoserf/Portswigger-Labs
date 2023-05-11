
# Information disclosure in error messages

This lab's verbose error messages reveal that it is using a vulnerable version of a third-party framework. To solve the lab, obtain and submit the version number of this framework.


---------------------------------------------

References: 

- https://portswigger.net/web-security/information-disclosure/exploiting



![img](images/Information%20disclosure%20in%20error%20messages/1.png)

---------------------------------------------

To generate an error I sent a string in the following request, which expects an integer:

```
GET /product?productId=aaa
```



![img](images/Information%20disclosure%20in%20error%20messages/2.png)