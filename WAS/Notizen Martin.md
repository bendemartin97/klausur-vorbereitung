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
- xml externel eintities (xxe):
	- dos attack, access external entites, like files, access can be extendend to files not local if attacksers know location and structure of web application
	- prevent:
		- upgrage xml processors and libraries, disable xml external entity and drd processing, whitelisting
- prevent:
	- hardening proces, remove all unnecessary features, updating configurations, segmentation, client security directives (security headers), automated verification process

### A06 - Vulnerable and Outdated Components
- exploite vulnerable component, like libraries, frameworks, which are running with the same privilages as the application
- common vulneralibities:
	- you do not know the versions of all components in use, know all outdated or vulnerable, unsupported software, scan for vulnerabilities regularly
- prevent:
	- remove unused dependencies, features, components, invertory the versions, using of components from official sources over secure links, monitor for libraries and components   

### A07 - Identification and Authentication Failures
- **Allows attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to assume other users’ identities temporarily or permanently**
- common vulneralibities:
	- weak passwords and forgotpassword process, weak hash functions for passwords, missing multi-factor auth, session identifier in url, not correct invalidated session ids
- prevent:
	- multi-factor auth, weak password checks, limit failed login attempts, log all failures, server-side session manager

### A08 - Software and Data Integrity Failures
- **Software and data integrity failures relate to code and infrastructure that does not protect against integrity violations**
- common vulneralibities:
	- using of libraries from untrusted sources, insecure CI/CD pipeline, auto-update of libraries without integrity verification, insecure deserialization
- prevent:
	- digital signatures to verify data, using components from trusted repos, review code and config changes, CI/CD pipeline with proper segregation, config, access control, do not sent unencrypted serialized data to untrusted client

### A09 - Security Logging and Monitoring Failures
- **his category is to help detect, escalate, and respond to active breaches**
- common vulneralibities:
	- logins, failed logins etc are not logged, warnings and errors generate unclear log messages, logs not monitored for suspicious activitiy, log store locally, useless alerting thresholds, testing do no trigger alerts, cannot detect active attacks in real-time
- prevent:
	- log login, validation failures, encode log data correctly, effective monitoring and alerting for suspicious activities

### A10 - Server-Side Request Forgery (SSRF)
- **SSRF flaws occur whenever a web application is fetching a remote resource without validating the user-supplied URL**
- CSRF:
	- force a victim browser with victon credentials, such session cookie, to generate requests
	- prevent:
		- using of an existing csrf defense, unpredictable token in each HTTP request

### ASVS
- application security standard with 3 Levels
- baseline: min. required for all app, mostly full testable and automatable
- Standard: suitable for sensitive data, 75% testable, somewhat automatable
- Comprehensive: suitable for critical app, mostly testabla via automation, manual verifacitions required
### C1- Define Security Requierments
- statement of needed security functionality

### C2 - Leverage Security Frameworks and Libraries
- help __guard against security-related desing and implementation flaws__
- because __missing of sufficient knowledge, time, or budget not sufficient security features__

### C3 - Secure DB Access
### C4 - Encode and Escape Data
- encoding: __translating special chars into__ different but __equivalent form__ (OWASP Java Encoder Project, MS Encoder and AntiXSS Library)

### C5 - Validate All Inputs
- syntax validity: data in in the expected form
- semantic validity: data is within an accaptable range for the given app functionality
- using of black or whitelist

### C6 - Implement Digital Identity
- unique representation of a user
- authentication: __process of verifying that an entity is who they claim to be__
- session management: __process by wich server maintains the state of the user authentication__

### C7 - Enforce Access Controls
- access control: __process of granting speicific requests from a user__ __and involves the act of granting and revoking those privileges__

### C8 - Protect Data Everywhere
- _benefits of HTTPS:_
	- __confidentiality__: attacker cannot view your data
	- __integrity__: attacker cannot change your data
	- __authenticity__: server you are visiting is the right one
- HSTS (Strict Transport Security)
	- __forces browser to only HTTPS connect to server__ (server sends Strict-Transport-Security-Header)
	- __fowarding secrecy__ ( negotiate Secretc throug an ephemeral key exchange)

### C9 - Sec. Logging und Monitoring
 detection points:
	- input validation __failure server-side when client-sode validation exists__
	- input validation __failure server-side on non-user editable parameters__
	- forced __browsing to common attack entry point__
	- __honeypot URL__ (e.g /admin/secretlogin.jsp)
	- __sql or xss attacks__

### C10 - Handle All Errors and Exceptions
- __do not lead critical data__
- __exceptions logged with enough information for:__
	- quantity assurance
	- forensics
	- incident response teams

Directoriy travelsat attack: an attacker is able to access data outside the web doc root, include scripts, read directories, which they could not read
Vertical privilege escalation: access resources granted to more priviliged accounts
SSL chiper suite is specified by: the encryption key lenght, hash algorithm, an encryption protocol
using site search operator to restrict search results to a spicific domain
SQL Injection attacks: inband, inferential or blind, out-of-band
out-of-band is useful, if there is no connection to the db
Merriam-Webster Dictionaray: to be assigned a standing or evaluation based on tests, to put to test or proof, to undergo a test
One of the best method to prevent sec bugs is to improve the SDLC by including security in each of its phases
Penetration testind requires a lower skill-seet than source code review
Automates application scanners for black box testing is not effective
HTTP Paramater pollution is to supply multiple http parameters with the same name in one request
Reflected XSS is: attacker injects browser executable code, is non-persistent and first-order or type 1 XSS, is not stored in the application