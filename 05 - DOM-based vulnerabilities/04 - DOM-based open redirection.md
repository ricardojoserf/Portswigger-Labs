
# DOM-based open redirection

This lab contains a DOM-based open-redirection vulnerability. To solve this lab, exploit this vulnerability and redirect the victim to the exploit server.

---------------------------------------------

References: 

- https://portswigger.net/web-security/dom-based/open-redirection



![img](images/DOM-based%20open%20redirection/1.png)

---------------------------------------------

The link to return to the home page from any post contains the following code:

```
<div class="is-linkback">
    <a href='#' onclick='returnUrl = /url=(https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "/"'>Back to Blog</a>
</div>
```



![img](images/DOM-based%20open%20redirection/2.png)


So when we hit “Back to Blog” it appends the “#” and returns to the home page:



![img](images/DOM-based%20open%20redirection/3.png)


The "location" object is the URL:



![img](images/DOM-based%20open%20redirection/4.png)


If there are matches for url=(https?:\/\/.+)/ in location then it redirects else it redirects to '/'


In this case I solved it with the query:

```
/post?postId=1&url=https://exploit-0acb008f03d717b7811033f6019200df.exploit-server.net/
```