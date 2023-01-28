### Same Origin Policy
- web browser permits scripts contained in a first web page to access data in a second web page, but _only if both web pages have the same origin_ (same URI scheme, host and port)

### Cross-orogin Resource Sharing
- _allows restricted resources on a web page to be requested from another domain_ outside the domain from which the first resource was served

### A01 - Broken Access Control
- moving up from the fifth position
- Access control enforces policy such that **users cannot act outside of their intended permissions**
- Common vulneralibities:
	- __if principle of least privilege is going to be violated, or a user can do something, for wich he does not have any permission__ (modifying request, missing access control for api, metadata manipulation)
- Prevent:
	- testing for access contol, input validation, enforce record ownership

### A02 - Cryptographic Failures
- moving from the third position
- **validation of certs, using of outdated cryptographic algorithms adn hash functions**
- prevent:
	- **don't store and cache sensitive data unnecessarily, using strong adaptive and salted hashing functions**

### A03 - Injection
- Common vulneralibities: 
	- **incoming data is not validated, dynamic queries are non-parameterized, hostile data is directly used**
- how to find vulneralibities:
	- **static code analysis**: part of CI, manual code reviews
	- **dynamic analysis**: vulnerability scanning, penetration testers
- SQL-Intection types:
	- in-band:
		- error-based: attackers get i**nformation about the structure of the db from the caused db error**
		- union-bases: fuses **multiple select statements to get a single http response**
	- blind:
		- from the **response and behavior can attackers derive the servers structure**
	- out-of-band
- Cross-Site Scripting:
	- The application uses untrusted data in the construction of the following HTML snippet _without validation or escaping_
- prevent:
	- using of sage api, parameterized interface, white listing, sql control statements, escape special characters 

### A04- Insecure desing
- insecure desing can not be fixed by perfect implementation
- in _user story development determine the correct flow and failure states_
- prevent:
	- using of secure dev lifecycle, library of secure design patterns, threat modelling, testing

### A05- Security Misconfiguration
- missing security hardening, unnecessary features are enabled, default accounts are enabled, error handling shows stack traces, latest security features are disabled, security settings are not set to secure values, missing security headers, software is outdated
- xml externel eintities:
	- dos attack, access external entites, like files, access can be extendend to files not local if attacksers know location and structure of web application
	- prevent:
		- upgrage xml processors and libraries, disable xml external entity and drd processing, whitelisting
- prevent:
	- hardening proces, remove all unnecessary features, updating configurations, segmentation, client security directives (security headers), automated verification process

### A06 - Vulnerable and Outdated Components
- exploite vulnerable component, like libraries, frameworks, which are running with the same privilages as the application
- common vulnerabilities:
	- you do not know the versions of all components in use, know all outdated or vulnerable, unsupported software, scan for vulnerabilities regularly
- prevent:
	- remove unused dependencies, features, components, invertory the versions, using of components from official sources over secure links, monitor for libraries and components   

### A07 - Identification and A