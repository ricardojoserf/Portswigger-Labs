
# DOM XSS in innerHTML sink using source location.search

This lab contains a DOM-based cross-site scripting vulnerability in the search blog functionality. It uses an innerHTML assignment, which changes the HTML contents of a div element, using data from location.search.

To solve this lab, perform a cross-site scripting attack that calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


There is a search function in "/?search=":



![img](images/DOM%20XSS%20in%20innerHTML%20sink%20using%20source%20location.search/1.png)


In the source code we see the sink:

```
function doSearchQuery(query) {
    document.getElementById('searchMessage').innerHTML = query;
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    doSearchQuery(query);
}
```



![img](images/DOM%20XSS%20in%20innerHTML%20sink%20using%20source%20location.search/2.png)


The HTML content of the searchMessage, a span HTML element, is generated from the content of the “search" GET parameter of the request. We can pop an alert with the payload:

```
<img src=x onerror=alert(1) />
```

![img](images/DOM%20XSS%20in%20innerHTML%20sink%20using%20source%20location.search/3.png)
