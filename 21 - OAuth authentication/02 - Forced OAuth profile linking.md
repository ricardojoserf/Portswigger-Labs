
# Forced OAuth profile linking

This lab gives you the option to attach a social media profile to your account so that you can log in via OAuth instead of using the normal username and password. Due to the insecure implementation of the OAuth flow by the client application, an attacker can manipulate this functionality to obtain access to other users' accounts.

To solve the lab, use a CSRF attack to attach your own social media profile to the admin user's account on the blog website, then access the admin panel and delete Carlos.

The admin user will open anything you send from the exploit server and they always have an active session on the blog website.

You can log in to your own accounts using the following credentials:

- Blog website account: wiener:peter

- Social media profile: peter.wiener:hotdog


---------------------------------------------

References: 

- https://portswigger.net/web-security/oauth



![img](images/Forced%20OAuth%20profile%20linking/1.png)

---------------------------------------------

Once authenticated, there is an options to attach a social profile:



![img](images/Forced%20OAuth%20profile%20linking/2.png)


It redirects to:



![img](images/Forced%20OAuth%20profile%20linking/3.png)


And then:



![img](images/Forced%20OAuth%20profile%20linking/4.png)


The last request is a GET request to “/oauth-linking” with a code:



![img](images/Forced%20OAuth%20profile%20linking/5.png)


Create an iframe for the victim to access this link:

```
<iframe src="https://0a0200d703e9d1e08553857b002e00df.web-security-academy.net/oauth-linking?code=jETtPOj5DHmBoJt_E3Po0I2PQEndgDoRhuFexNBuDpt"></iframe>
```



![img](images/Forced%20OAuth%20profile%20linking/6.png)


Then click “Log in with social media” and you will log in as the administrator:



![img](images/Forced%20OAuth%20profile%20linking/7.png)