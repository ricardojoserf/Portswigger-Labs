
# DOM XSS in jQuery anchor href attribute sink using location.search source

This lab contains a DOM-based cross-site scripting vulnerability in the submit feedback page. It uses the jQuery library's $ selector function to find an anchor element, and changes its href attribute using data from location.search.

To solve this lab, make the "back" link alert document.cookie.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


This is the sink in the "Submit Feedback" page:



![img](images/DOM%20XSS%20in%20jQuery%20anchor%20href%20attribute%20sink%20using%20location.search%20source/1.png)


And the url of the "Submit Feedback" page is https://0af5007903b0426b803b4e9100cb0023.web-security-academy.net/feedback?returnPath=/:



![img](images/DOM%20XSS%20in%20jQuery%20anchor%20href%20attribute%20sink%20using%20location.search%20source/2.png)

It is possible to use a Javascript url like:

```
/feedback?returnPath=javascript:alert(document.cookie)
```




![img](images/DOM%20XSS%20in%20jQuery%20anchor%20href%20attribute%20sink%20using%20location.search%20source/3.png)
