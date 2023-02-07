### Same Origin Policy
- web browser permits scripts contained in a first web page to access data in a second web page, but _only if both web pages have the same origin_ (same URI scheme, host and port)

### Cross-orogin Resource Sharing
- _allows restricted resources on a web page to be requested from another domain_ outside the domain from which the first resource was served

## Owasp Top Ten

### A01: Broken Access Control
- moving up from the fifth position
-   Access control enforces policy such that users cannot act outside of their intended permissions
-   Common vulnerabilities: 
	- violation of the principle of least privilege, 
	- missing access control for APIs, 
	- metadata manipulation
-   Prevent: 
	- testing for access control, 
	- input validation, 
	- enforcing record ownership

### A02: Cryptographic Failures
- moving from the third position
-  Validation of certificates, use of outdated cryptographic algorithms and hash functions
-  Prevent: don't store and cache sensitive data unnecessarily, use strong adaptive and salted hashing functions

### A03: Injection

-   Common vulnerabilities: incoming data not validated, dynamic queries not parameterized, hostile data directly used
-   Find vulnerabilities: static code analysis (part of CI, manual code reviews), dynamic analysis (vulnerability scanning, penetration testing)
-   Prevent: use of safe APIs, parameterized interfaces, white listing, escape special characters

### A04: Insecure Design

-   Insecure design cannot be fixed by perfect implementation
-   Prevent: use of secure dev life cycle, library of secure design patterns, threat modeling, testing

### A05: Security Misconfiguration

-   Common vulnerabilities: missing security hardening, unnecessary features enabled, default accounts enabled, error handling shows stack traces, security settings not set to secure values, missing security headers, outdated software
-   Prevent: hardening process, remove all unnecessary features, updating configurations, segmentation, client security directives (security headers), automated verification process

### A06: Vulnerable and Outdated Components

-   Exploiting vulnerable components like libraries, frameworks that run with the same privileges as the application
-   Common vulnerabilities: outdated or vulnerable software, unsupported software
-   Prevent: remove unused dependencies, features, components, inventory the versions, use components from official sources over secure links, monitor for libraries and components

### A07: Identification and Authentication Failures

-   Allows attackers to compromise passwords, keys, or session tokens, or exploit implementation flaws to assume other users’ identities temporarily or permanently
-   Common vulnerabilities: weak passwords, weak hash functions, missing multi-factor authentication, session identifier in URL
-   Prevent: multi-factor authentication, limit failed login attempts, log all failures, server-side session manager

### A08: Software and Data Integrity Failures

-   Software and data integrity failures relate to code and infrastructure that does not protect against integrity violations
-   Common vulnerabilities: using libraries from untrusted sources, insecure CI/CD pipeline, auto-update of libraries without integrity verification
-   Prevent: digital signatures to verify data, use components from trusted repos, review code and config changes, CI/CD pipeline with proper segregation, config, access control, don't send unencrypted serialized data to untrusted client

### A09: Security Logging and Monitoring Failures

-   This category is to help detect, escalate, and respond to active breaches
-   Common vulnerabilities: logins, failed logins, etc. not logged, unclear log messages
-   Prevent: log all security-relevant events, implement logging and alerting, review logs regularly.

### A10 - Broken Function Level Access Control

-   A vulnerability in which access controls are not properly restricted at the function level, allowing attackers to bypass intended restrictions and access sensitive resources
-   Common vulnerabilities:
    -   weak access controls, missing or inadequate access control checks, user-controlled input used as direct or indirect access control decision factor
-   Prevent:
    -   implement least privilege principle, server-side access control checks, validate user input, log and monitor access control decisions and violations, encrypt sensitive data


## ASVS
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
	- __forces browser to only HTTPS (never HTTP) connect to server__ (server sends Strict-Transport-Security-Header)
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

## Notes

**Vertical privilege escalation**: 
- access resources granted to more priviliged accounts SSL chiper suite is specified by: the encryption key lenght, hash algorithm, an encryption protocol using site search operator to restrict search results to a spicific domain
**SQL Injection attacks**: 
- inband, inferential or blind, out-of-band
- out-of-band is useful, if there is no connection to the db
**Merriam-Webster Dictionaray**: 
- to be assigned a standing or evaluation based on tests, to put to test or proof, to undergo a test

> One of the best method to prevent sec bugs is to improve the SDLC by including security in each of its phases


> Penetration testind requires a lower skill-seet than source code review


> Automate application scanners for black box testing is not effective

> HTTP Paramater pollution is to supply multiple http parameters with the same name in one request

> Reflected XSS is: attacker injects browser executable code, is non-persistent and first-order or type 1 XSS, is not stored in the application inc and .asa files never returned from a web server