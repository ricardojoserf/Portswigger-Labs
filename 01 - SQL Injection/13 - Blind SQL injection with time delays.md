# Blind SQL injection with time delays

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

To solve the lab, exploit the SQL injection vulnerability to cause a 10 second delay.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/blind

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------



There is a SQL injection in the cookie. It uses PostgreSQL, sleep 10 seconds with:

```
COOKIE'||pg_sleep(10)--
```

```
TGjY2hbNNRAamLIb'||pg_sleep(10)--
```



![img](images/Blind%20SQL%20injection%20with%20time%20delays/1.png)

