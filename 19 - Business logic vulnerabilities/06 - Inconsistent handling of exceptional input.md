
# Inconsistent handling of exceptional input

This lab doesn't adequately validate user input. You can exploit a logic flaw in its account registration process to gain access to administrative functionality. To solve the lab, access the admin panel and delete Carlos.

Hint: You can use the link in the lab banner to access an email client connected to your own private mail server. The client will display all messages sent to @YOUR-EMAIL-ID.web-security-academy.net and any arbitrary subdomains. Your unique email ID is displayed in the email client.


---------------------------------------------

References: 

- https://portswigger.net/web-security/logic-flaws/examples



![img](images/Inconsistent%20handling%20of%20exceptional%20input/1.png)

---------------------------------------------

We can not register as the carlos user as it already exists. We register as the user “test” with email “attacker@exploit-0a67006c0423355d834f36b001cf0030.exploit-server.net”:



![img](images/Inconsistent%20handling%20of%20exceptional%20input/2.png)



We can not access /admin:



![img](images/Inconsistent%20handling%20of%20exceptional%20input/3.png)


Using a very long subdomain:



![img](images/Inconsistent%20handling%20of%20exceptional%20input/4.png)


The email domain is 246 “a” characters:



![img](images/Inconsistent%20handling%20of%20exceptional%20input/5.png)



With the following email:

```
POST /register HTTP/2
...

csrf=alQnzW8wE9JhzqQtr4xigsVi409cfLqF&username=test15&email=attackeraaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa%40dontwannacry.com.exploit-0a67006c0423355d834f36b001cf0030.exploit-server.net&password=test15
```



![img](images/Inconsistent%20handling%20of%20exceptional%20input/6.png)


The domain is dontwannacry.com because the rest of the subdomain is cropped:



![img](images/Inconsistent%20handling%20of%20exceptional%20input/7.png)


And we can access /admin:



![img](images/Inconsistent%20handling%20of%20exceptional%20input/8.png)

