
# SSRF with blacklist-based input filter

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.

The developer has deployed two weak anti-SSRF defenses that you will need to bypass.


---------------------------------------------

References: 

- https://portswigger.net/web-security/ssrf



![img](images/SSRF%20with%20blacklist-based%20input%20filter/1.png)

---------------------------------------------


There is a button to check the stock:



![img](images/SSRF%20with%20blacklist-based%20input%20filter/2.png)


It generates this POST request:



![img](images/SSRF%20with%20blacklist-based%20input%20filter/3.png)


The stockApi parameter contains the url “http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1”, and we can tryo to change it to other url:



![img](images/SSRF%20with%20blacklist-based%20input%20filter/4.png)


It seems we can not use “http://127.0.0.1” or “http://localhost/”:



![img](images/SSRF%20with%20blacklist-based%20input%20filter/5.png)


Using http://017700000001/ the error changes:



![img](images/SSRF%20with%20blacklist-based%20input%20filter/6.png)


Using http://127.1 we can access the local server, for example we can access /login. But we still can not access /admin, it gets detected.



![img](images/SSRF%20with%20blacklist-based%20input%20filter/7.png)


Using case variation, using ADMIN in uppercase, it is possible to access /admin:



![img](images/SSRF%20with%20blacklist-based%20input%20filter/8.png)


From the source code we find it is possible to delete the user with a GET request with a payload like this:

```
POST /product/stock HTTP/2
...

stockApi=http://127.1/ADMIN/delete?username=carlos
```



![img](images/SSRF%20with%20blacklist-based%20input%20filter/9.png)