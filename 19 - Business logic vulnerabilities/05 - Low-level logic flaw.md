
# Low-level logic flaw

This lab doesn't adequately validate user input. You can exploit a logic flaw in its purchasing workflow to buy items for an unintended price. To solve the lab, buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: wiener:peter

Hint: You will need to use Burp Intruder (or Turbo Intruder) to solve this lab.

To make sure the price increases in predictable increments, we recommend configuring your attack to only send one request at a time. In Burp Intruder, you can do this from the resource pool settings using the Maximum concurrent requests option.

---------------------------------------------

References: 

- https://portswigger.net/web-security/logic-flaws/examples



![img](images/Low-level%20logic%20flaw/1.png)

---------------------------------------------

Add item request:



![img](images/Low-level%20logic%20flaw/2.png)


Checkout request:



![img](images/Low-level%20logic%20flaw/3.png)


Which gets a redirection to:



![img](images/Low-level%20logic%20flaw/4.png)


16041 items cost 2 million dollars:



![img](images/Low-level%20logic%20flaw/5.png)


16081 items cost -2 million dollars:



![img](images/Low-level%20logic%20flaw/6.png)


- 2^31 − 1 = 2.147.483.647, the maximum value in the backend for an integer. 

- 2.147.483.647 / 1337 = 16061 items we could add before this problem happens





![img](images/Low-level%20logic%20flaw/7.png)
![img](images/Low-level%20logic%20flaw/8.png)



To reach a value next to 0, we will try with 16061\*2 = 32122


![img](images/Low-level%20logic%20flaw/9.png)


As we were close enough, I added a different item to get a value between 0 and 100 dollars:



![img](images/Low-level%20logic%20flaw/10.png)


------------------

### Official solution

Intruder for quantity:



![img](images/Low-level%20logic%20flaw/11.png)

Using “Null payloads”:



![img](images/Low-level%20logic%20flaw/12.png)


And “Maximum concurrent threads” to 1:





![img](images/Low-level%20logic%20flaw/13.png)
![img](images/Low-level%20logic%20flaw/14.png)
