
# Information disclosure on debug page

This lab contains a debug page that discloses sensitive information about the application. To solve the lab, obtain and submit the SECRET_KEY environment variable.

---------------------------------------------

References: 

- https://portswigger.net/web-security/information-disclosure/exploiting



![img](images/Information%20disclosure%20on%20debug%20page/1.png)

---------------------------------------------

We find the debug page in “/cgi-bin/phpinfo.php”:



![img](images/Information%20disclosure%20on%20debug%20page/2.png)


We can find the secret key ("8f4xrr692ckcxycofkaupwwu37cse6io") in the “Environment” section:



![img](images/Information%20disclosure%20on%20debug%20page/3.png)
