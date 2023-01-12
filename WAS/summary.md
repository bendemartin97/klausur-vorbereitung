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
- [ ] Often checked using OPTIONS method before actual Request
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

### [A01 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) @Fabian
- Access control enforces policy such that __users cannot act outside of their intended permissions __![[Pasted image 20221219120548.png|500]]
- __Failures__ typically lead to:
    - Unauthorised information disclosure, modification, or destruction of all data or
    - performing a business function outside the user's limits
####    Common vulnerabilities
- 
### [A02 - Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)
### [A03 - Injection](https://owasp.org/Top10/A03_2021-Injection/)
### [A04 - Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/) 
### [A05 - Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) 
### [A06 - Vulnerable and Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/)  
### [A07 - Identification and Authentication Failures](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/)
### [A08 - Software and Data Integrity Failures](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/)
### [A09 - Security Logging and Monitoring Failures](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/)
### [A10 - Server-Side Request Forgery (SSRF)](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/)
### [Further risks (from earlier OWASP top 10 lists)](https://owasp.org/Top10/A11_2021-Next_Steps/)
