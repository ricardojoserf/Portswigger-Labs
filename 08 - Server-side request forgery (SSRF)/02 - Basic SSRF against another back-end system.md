
# Basic SSRF against another back-end system

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal 192.168.0.X range for an admin interface on port 8080, then use it to delete the user carlos.

---------------------------------------------

Generated link: https://0aac00f403f0604981937fdd00f60012.web-security-academy.net/

POST request to check stock:



![img](images/Basic%20SSRF%20against%20another%20back-end%20system/1.png)


Intruder:



![img](images/Basic%20SSRF%20against%20another%20back-end%20system/2.png)


.169 contains admin panel:



![img](images/Basic%20SSRF%20against%20another%20back-end%20system/3.png)


It seems it is possible to delete users with a GET request from the response:



![img](images/Basic%20SSRF%20against%20another%20back-end%20system/4.png)


We will add this to the request:



![img](images/Basic%20SSRF%20against%20another%20back-end%20system/5.png)


```
POST /product/stock HTTP/2
...
stockApi=http%3A%2F%2F192.168.0.169%3A8080%2Fadmin/delete?username=carlos
```

