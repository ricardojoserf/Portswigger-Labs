
# Reflected XSS in canonical link tag

This lab reflects user input in a canonical link tag and escapes angle brackets.

To solve the lab, perform a cross-site scripting attack on the home page that injects an attribute that calls the alert function.

To assist with your exploit, you can assume that the simulated user will press the following key combinations:

ALT+SHIFT+X
CTRL+ALT+X
Alt+X

Please note that the intended solution to this lab is only possible in Chrome.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts

- https://portswigger.net/research/xss-in-hidden-input-fields








![img](images/Reflected%20XSS%20in%20canonical%20link%20tag/1.png)
![img](images/Reflected%20XSS%20in%20canonical%20link%20tag/2.png)
![img](images/Reflected%20XSS%20in%20canonical%20link%20tag/3.png)

---------------------------------------------

The page allows to post comments:



![img](images/Reflected%20XSS%20in%20canonical%20link%20tag/4.png)


We find the link with 'rel="canonical"' in the head section of the HTML page:



![img](images/Reflected%20XSS%20in%20canonical%20link%20tag/5.png)


We would like to turn it to:

```
<link rel="canonical" accesskey="X" onclick="alert(1)" />
```

In the /post endpoint it is necessary to send a correct postId, but it is possible to add more parameters which change the content of the href attribute:



![img](images/Reflected%20XSS%20in%20canonical%20link%20tag/6.png)


A correct payload:

```
/post?postId=1&a=b'accesskey='X'onclick='alert(1)
```






![img](images/Reflected%20XSS%20in%20canonical%20link%20tag/7.png)
![img](images/Reflected%20XSS%20in%20canonical%20link%20tag/8.png)
