
# Source code disclosure via backup files

This lab leaks its source code via backup files in a hidden directory. To solve the lab, identify and submit the database password, which is hard-coded in the leaked source code.

---------------------------------------------

References: 

- https://portswigger.net/web-security/information-disclosure/exploiting



![img](images/Source%20code%20disclosure%20via%20backup%20files/1.png)

---------------------------------------------

There is a /robots.txt file:



![img](images/Source%20code%20disclosure%20via%20backup%20files/2.png)


There is a /backup endpoint:



![img](images/Source%20code%20disclosure%20via%20backup%20files/3.png)


We can read the file and fine the database password “td510drfnep124ibalwc0xw32d1cp3or”:



![img](images/Source%20code%20disclosure%20via%20backup%20files/4.png)