
# 21 - Information disclosure in version control history

This lab discloses sensitive information via its version control history. To solve the lab, obtain the password for the administrator user then log in and delete Carlos's account.

---------------------------------------------

Reference: https://portswigger.net/web-security/information-disclosure/exploiting



![img](images/21%20-%20Information%20disclosure%20in%20version%20control%20history/1.png)

---------------------------------------------

Generated link: https://0afa00c404fb4d8581880c60002e004e.web-security-academy.net/

The directory .git/ exists and allows directory listing:



![img](images/21%20-%20Information%20disclosure%20in%20version%20control%20history/2.png)

Download the gitdumper script (https://raw.githubusercontent.com/internetwache/GitTools/master/Dumper/gitdumper.sh) and then the .git repo:

```
bash gitdumper.sh https://0afa00c404fb4d8581880c60002e004e.web-security-academy.net/.git/ /tmp/a/
```



![img](images/21%20-%20Information%20disclosure%20in%20version%20control%20history/3.png)

There are sensitive files deleted in the latest commit:

```
cd /tmp/a
git status
```



![img](images/21%20-%20Information%20disclosure%20in%20version%20control%20history/4.png)

Then we revert the previous commit and read the file:

```
git log
git reset --hard d3e84943424222ce64de7da0d797b7dfdef39ea1
cat admin.conf
```



![img](images/21%20-%20Information%20disclosure%20in%20version%20control%20history/5.png)

Then we access with credentials administrator:wy0szsn75q5jqn0w70bu and delete the user:



![img](images/21%20-%20Information%20disclosure%20in%20version%20control%20history/6.png)
