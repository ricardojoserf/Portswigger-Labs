
# Infinite money logic flaw

This lab has a logic flaw in its purchasing workflow. To solve the lab, exploit this flaw to buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: wiener:peter

---------------------------------------------

References: 

- https://portswigger.net/web-security/logic-flaws/examples



![img](images/Infinite%20money%20logic%20flaw/1.png)

---------------------------------------------

It is possible to sign up for the newsletter:



![img](images/Infinite%20money%20logic%20flaw/2.png)


It returns a coupon:



![img](images/Infinite%20money%20logic%20flaw/3.png)


It is possible to use the coupon but only once:



![img](images/Infinite%20money%20logic%20flaw/4.png)


Request to purchase the item “Gift card”:



![img](images/Infinite%20money%20logic%20flaw/5.png)


It returns a code:



![img](images/Infinite%20money%20logic%20flaw/6.png)


This code can be redeemed and get money (we get more than the initial 100 dollars):



![img](images/Infinite%20money%20logic%20flaw/7.png)


We will get as many as possible gift cards with the current money and use intruder to validate all the codes:



![img](images/Infinite%20money%20logic%20flaw/8.png)


Continue the process, getting a total of (Total money % 7) gift cards and redeeming them until you get more than 1000 dollars:



![img](images/Infinite%20money%20logic%20flaw/9.png)