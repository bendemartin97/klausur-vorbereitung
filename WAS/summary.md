## Chapter 1

### Web Application structure
 Usually multi-tiered approach -> often __3 tiers__
 - presentation (web browser)
 - application logic (web server
 - storage (database)![[Pasted image 20221216112016.png]]

### Server-side Architecture
####    Stand-alone Architecture
   - Web application is a stand-alone binary program
   - Program restarted for each request
####    Integrated  Architecture
   - Web application is part of the web server
   - No need to restart for each request
   - E. g. PHP, Perl, Python

### Reducing load on the Server
####    Single Page Application
   - SPA's __dynamically rewrite the current web page__ with new data from the web server (instead of loading  a complete new page)
   - Server delivers JavaScript, CSS etc. via web services, e.g REST API
   - __Calculations are performed on the client__ ![[Pasted image 20221216120858.png]]
   
### HTTP Proxies
####    Forwarding
   - acts as __intermediary for clients requesting resources__ from a web server ![[Pasted image 20221216121124.png]]
   - _additional services_
       - Caching
       - Authentication
       - Access Control
####    Reverse
   - acts as intermediary for origin web servers to which clients connect ![[Pasted image 20221216121510.png]]
   - _advantages_
       - load balancing 
       - TLS (SSL) tasks
       - protection
       - caching

## Chapter 2

### HTTP Request & Response
- .....

###  Same Origin Policy (SOP)
- web browser permits scripts contained in a first web page to access data in a second web page, but _only if both web pages have the same origin_, i.e. the same URI scheme (like http or https), host name and port number
- Third-party scripts may access user information on frontend (cookies, local storage) or backend (banking, email, network devices) => __security risk__
- Sometimes, however, pages would like to allow other pages to access their data... => __CORS__

###  Cross-origin Resource Sharing (CORS)
- is a mechanism that _allows restricted resources on a web page to be requested from another domain_ outside the domain from which the first resource was served
- CORS Headers in HTTP Requests and Resources allow server to define SOP exceptions
    - CORS Request: Origin [host name]  
    - CORS Response: Access-Control-Allow-Origin [host name]
    - ....
- Often checked using OPTIONS method before actual Request
    - so-called _CORS preflight request_ to avoid errors

### URI vs. URL vs. URN
- _Uniform Resource Identifiers (URIs)_ are a superset of => e.g  www.fh-aachen.de
    - _Uniform Resource Locators (URLs)_ => e.g  http://www.fh-aachen.de
        - Protocol://hostname[:port]/[path]file[?param=value] 
        - https://www.fh-aachen.de/auth/448/Details.ashx?uid=129
    - _Uniform Resource Names (URNs)_
        - persistent, location-independent, resource identifiers
        - one of a hundred URI schemes (“urn:”, “http:”, “ftp:” ...)

### Status Codes HTTP
- 1xx – Informational responses
- 2xx – Success  
- 3xx – Redirection  
- 4xx – Client errors
- 5xx – Server errors

## Chapter 3

### [Agenda OWASP Top 10](https://owasp.org/Top10/)
    [[#A01 - Broken Access Control]]
    [[#A02 - Cryptographic Failures  ]]
    [[#A03 - Injection]]
    [[#A04 - Insecure Design]]
    [[#A05 - Security Misconfiguration]]  
    [[#A06 - Vulnerable and Outdated Components]]  
    [[#A07 - Identification and Authentication Failures]]
    [[#A08 - Software and Data Integrity Failures]]  
    [[#A09 - Security Logging and Monitoring Failures]]
    [[#A10 - Server-Side Request Forgery (SSRF)]]
    [[#Further risks (from earlier OWASP top 10 lists)]]

### A01 - Broken Access Control 
[Link](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

- Access control enforces policy such that __users cannot act outside of their intended permissions__  ![[Pasted image 20221219120548.png|500]]
- __Failures__ typically lead to:
    - Unauthorised information disclosure, modification, or destruction of all data or
    - performing a business function outside the user's limits
#### Common vulnerabilities
   - __Violation of principle of least privilege__ or __deny by default__
	   - access ***should*** only be granted for particular capabilities, roles, or users, but is available to anyone
   - __Bypassing access control checks__ by  
	   - modifying the URL (parameter tampering or force browsing), internal application state, or the HTML page, or  
	   - by using an attack tool modifying API requests
   - __Permitting viewing or editing__ someone else's account,  
	   - by providing its unique identifier (insecure direct object references)
   - Accessing API with __missing access controls for POST, PUT and DELETE__
   - __Elevation of privilege__  
	   - acting as a user without being logged in or  
	   - acting as an admin when logged in as a user
   - __Metadata manipulation__, such as
	   - replaying or tampering with a JSON Web Token (JWT) access control token, or
	   - a cookie or hidden field manipulated to elevate privileges or
	   - abusing JWT invalidation
   - __CORS misconfiguration__ allows API access from unauthorized/untrusted origins
   - __Force browsing__ 
	   - to authenticated pages as an unauthenticated user or
	   - to privileged pages as a standard user

#### Example Attacks
- _Scenario #1:_ The application uses unverified data in a SQL call that is accessing account information:
``` php
pstmt.setString(1, request.getParameter(“acct”)); ResultSet results = pstmt.executeQuery();
```
An attacker simply __modifies the ‘acct’ parameter in the browser to send whatever account number they want__. If not properly verified, the attacker can access any user’s account.
	http://example.com/app/accountInfo?acct=notmyacct

#### How to prevent
- __Deny by default__
- __Enforce record ownership__
- __Input validation__
- __functional access control unit and integration tests__

#####    Insecure Direct Object References
   - Attacker changes account id in Url to get acces to other accounts
	   - ?acct=6065 -> 6064
   
### A02 - Cryptographic Failures 
[Link](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

### A03 - Injection 
[Link](https://owasp.org/Top10/A03_2021-Injection/)

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

### A04 - Insecure Design 
[Link](https://owasp.org/Top10/A04_2021-Insecure_Design/) 

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

### A05 - Security Misconfiguration 
[Link](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) 

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

### A06 - Vulnerable and Outdated Components 
[Link](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/)  

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

### A07 - Identification and Authentication Failures 
[Link](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/)

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

### A08 - Software and Data Integrity Failures 
[Link](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/)

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

### A09 - Security Logging and Monitoring Failures 
[Link](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/)

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

### A10 - Server-Side Request Forgery (SSRF) 
[Link](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/)

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

### Further risks (from earlier OWASP top 10 lists) 
[Link](https://owasp.org/Top10/A11_2021-Next_Steps/)

#### Common vulnerabilities
#### Example Attacks
#### How to prevent 

## Chapter 4

### OWASP Top Ten Proactive Controls (OPCs)
	[[#C1 - Define Security Requirements]]
	[[#C2 - Leverage Security Frameworks and Libraries]]
	[[#C3 - Secure Database Access  ]]
	[[#C4 - Encode and Escape Data  ]]
	[[#C5 - Validate All Inputs  ]]
	[[#C6 - Implement Digital Identity]]
	[[#C7 - Enforce Access Controls  ]]
	[[#C8 - Protect Data Everywhere ]] 
	[[#C9 - Implement Security Logging and Monitoring]]
	[[#C10 - Handle All Errors and Exceptions]]


### C1 - Define Security Requirements  
### C2 - Leverage Security Frameworks and Libraries
### C3 - Secure Database Access  
### C4 - Encode and Escape Data  
### C5 - Validate All Inputs  
### C6 - Implement Digital Identity  
### C7 - Enforce Access Controls  
### C8 - Protect Data Everywhere  
### C9 - Implement Security Logging and Monitoring
### C10 - Handle All Errors and Exceptions

## Chapter 5

### Application Security Verification Standard (ASVS)
	[[#Level 1: Opportunistic (Minimum Requirement)]]
	[[#Level 2: Standard (Normal Requirements)]]
	[[#Level 3: Advanced (Maximum Requirements)]]
	[[#Applying ASVS in Practice]]


### Level 1: Opportunistic (Minimum Requirement)
### Level 2: Standard (Normal Requirements)
### Level 3: Advanced (Maximum Requirements)
### Applying ASVS in Practice
