
# Routing-based SSRF

This lab is vulnerable to routing-based SSRF via the Host header. You can exploit this to access an insecure intranet admin panel located on an internal IP address.

To solve the lab, access the internal admin panel located in the 192.168.0.0/24 range, then delete Carlos.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.

---------------------------------------------

References: 

- https://portswigger.net/web-security/host-header/exploiting



![img](images/Routing-based%20SSRF/1.png)

---------------------------------------------


Intercept a request to “/” and change the “Host” header for a Burp collaborator url:



![img](images/Routing-based%20SSRF/2.png)


The domain is resolved:



![img](images/Routing-based%20SSRF/3.png)


Then send the request to Intruder:



![img](images/Routing-based%20SSRF/4.png)


We find an admin panel at 192.168.0.127:



![img](images/Routing-based%20SSRF/5.png)


It seems it is a POST request:



![img](images/Routing-based%20SSRF/6.png)


But I tried a GET request and it works as well:



![img](images/Routing-based%20SSRF/7.png)
