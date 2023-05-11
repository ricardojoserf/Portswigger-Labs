
# 17 - Insufficient workflow validation

This lab makes flawed assumptions about the sequence of events in the purchasing workflow. To solve the lab, exploit this flaw to buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

Generated link: https://0a44005d0489eb06819b441c00f2003f.web-security-academy.net/


First I will try to buy some eggs:



![img](images/17%20-%20Insufficient%20workflow%20validation/1.png)

Clicking "Place order" generates a POST request:



![img](images/17%20-%20Insufficient%20workflow%20validation/2.png)

And a GET request to "/cart/order-confirmation?order-confirmed=true":



![img](images/17%20-%20Insufficient%20workflow%20validation/3.png)

After adding the leather jacket and clicking "Place order" you get a POST request and a GET request but to "/cart?err=INSUFFICIENT_FUNDS":



![img](images/17%20-%20Insufficient%20workflow%20validation/4.png)

If you substitute that with "/cart/order-confirmation?order-confirmed=true" the order is created:



![img](images/17%20-%20Insufficient%20workflow%20validation/5.png)



![img](images/17%20-%20Insufficient%20workflow%20validation/6.png)
