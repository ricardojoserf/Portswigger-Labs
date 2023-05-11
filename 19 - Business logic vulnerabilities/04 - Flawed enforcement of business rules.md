
# Flawed enforcement of business rules

This lab has a logic flaw in its purchasing workflow. To solve the lab, exploit this flaw to buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/logic-flaws/examples



![img](images/Flawed%20enforcement%20of%20business%20rules/1.png)

---------------------------------------------

There is a code for new users:



![img](images/Flawed%20enforcement%20of%20business%20rules/2.png)


It is applied with a POST request:



![img](images/Flawed%20enforcement%20of%20business%20rules/3.png)



But the second time the response is:



![img](images/Flawed%20enforcement%20of%20business%20rules/4.png)



There is a sign up field:



![img](images/Flawed%20enforcement%20of%20business%20rules/5.png)


It returns a new coupon:



![img](images/Flawed%20enforcement%20of%20business%20rules/6.png)



We can use this coupon as well:



![img](images/Flawed%20enforcement%20of%20business%20rules/7.png)



We can not use the same coupon twice in a row but we can use once each until we get a full discount:



![img](images/Flawed%20enforcement%20of%20business%20rules/8.png)