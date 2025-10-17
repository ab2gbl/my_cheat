
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
# FrontEnd 
## Cross-Site Scripting (XSS)
- **3 types**:
```javascript
#"><img src=/ onerror=alert(document.cookie)>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NDMyODgwNzUsMTk2NTg1Njg2Ml19
-->