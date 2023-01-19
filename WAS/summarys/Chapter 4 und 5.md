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
- security requirements: __statement of needed security functionality__
	- derived from industry standards
	- define new features to solve specific security problem or elimante vulnerability
	- provide a foundation for security
- standard security requirements for reusing 
	- prevent the repeat of past security failures
- Application Security Verifactiion Standard 4.0.1:
	- first application security standard 
	- defines __3 risk levels with 280 controls__
	- _3 Levels:_
		1. Baseline: min. required for all app, mostly fully testable and automatable
		2. Standard: suitable for sensitive data, 75% testable, somewhat automatable
		3. Comprehensive: suitable for critical app, mostly testabla via automation, manual verifacitions required
- __includes the writing of unit tests__
	- __validate your app__ each and every build
	- allows to concentrate on difficult to automate test (business logic flaws, access control issues etc.)
### C2 - Leverage Security Frameworks and Libraries
- help __guard against security-related desing and implementation flaws__
- because __missing of sufficient knowledge, time, or budget not sufficient security features__
- Best practices:
	- use existing coding libraries from trusted sources 
	- use native secure features of frameworks
	- stay and keep libraries and components up to date
	- expose only the requires behaviour into your software
### C3 - Secure Database Access 
- validate passwords
- use prepateStatements
### C4 - Encode and Escape Data 
- encoding: __translating special chars into__ different but __equivalent form__
	- HTML: "<" -> "&lt;"
- escaping: __adding a special char__ before the char / string aviod it being misinterpreted
- _Solutions:_
	- OWASP Java Encoder Project
		- designed for high-availability, high-perfomance encoding
		- no configuration
		- no dependencies
	- Microsoft Encoder and AntiXSS Library
		- can encode for different contexts : html, xml, css, JS
### C5 - Validate All Inputs  
- validate input before using it
	- __syntax validity: data in in the expected form__ 
		- example: app allows to enter 4 digit account id -> app checks that the entered data is exactly 4 digits and ionly numbers
	- __semantic validity: data is within an accaptable range for the given application functionality__ 
		- example: start date is before end date
- _two general approaches:_
	1. blacklisting:
		- __check that data does not contain "know bad" content__
	2. whitelisting:
		- __check that data matches a set of "know good" content__
### C6 - Implement Digital Identity  
- digital identity: __unique representation of a user__
- authentication: __process of verifying that an entity is who they claim to be__
- session management: __process by wich server maintains the state of the user authentication__ -> user has not to reauthenticate
- _Best Practices:_
	- do not limit the password length and do not allow common passwords
	- use a cryptograpiclly strong credential-specific salt (32+ byte)
	- impose difficult verifaction on only the attacker
### C7 - Enforce Access Controls 
- access control: __process of granting speicific requests from a user__ __and involves the act of granting and revoking those privileges__
- seperate application and access control code
- default deny
### C8 - Protect Data Everywhere  
- _benefits of HTTPS:_
	- __confidentiality__: attacker cannot view your data
	- __integrity__: attacker cannot change your data
	- __authenticity__: server you are visiting is the right one
- HSTS (Strict Transport Security)
	- __forces browser to only HTTPS connect to server__ (server sends Strict-Transport-Security-Header)
	- __fowarding secrecy__ ( negotiate Secretc throug an ephemeral key exchange)
### C9 - Implement Security Logging and Monitoring
- logging: concept for debugging and diagnostic purposes
- monitoring: live review of application and security logs using automation
- __use common standard frameworks__
- __protect againts log injection attacks__
- detection points:
	- input validation __failure server-side when client-sode validation exists__
	- input validation __failure server-side on non-user editable parameters__
	- forced __browsing to common attack entry point__
	- __honeypot URL__ (e.g /admin/secretlogin.jsp)
	- __sql or xss attacks__
### C10 - Handle All Errors and Exceptions
- exception handling: concept that allows an application to respond to different error states
- manage exceptions in a centralized manner
- __do not lead critical data__
- __exceptions logged with enough information for:__
	- quantity assurance
	- forensics
	- incident response teams

# Chapter 5
### Application Security Verification Standard (ASVS)
	[[#Level 1: Opportunistic (Minimum Requirement)]]
	[[#Level 2: Standard (Normal Requirements)]]
	[[#Level 3: Advanced (Maximum Requirements)]]
	[[#Applying ASVS in Practice]]

- list of application security requirements or tests
- can be used to define what a secure application is
- _two main goals:_
	- to help organizations develop and maintain secure applications
	- to allow security service vendors, tool and consumers to align their requirements and offerings
- use it as a blueprint
- create secure coding checklist
- tailor asvs to your use case

### Level 1: Opportunistic (Minimum Requirement)
- application achives, if it adequately defends against security vulnerabilities, that are __easy to discover or included in the OWASP Top 10__
- verification:
	- automatically by tools
	- simply manually
	- without access to source code (black box)
- Threat agents focus on vulneralibities, which are:
	- easy to find
	- easy to exploit

### Level 2: Standard (Normal Requirements)
- application achives, if it adequately __defends against most of the risks associated with software today__
- for applications, __that handle significant b2b transactions__
- Threat agents will typically:
	- skilled & motivated attackers 
	- use tools & techniques effective

### Level 3: Advanced (Maximum Requirements)
- applications achives, if __it adequately defends against advanced application security vulnerabilities__ 
- for applications with
	- required significant security level
	- perform ciritical functions
- demostrate a __good security design__ ( modularized, each module seperated by network etc.)

### Applying ASVS in Practice
- __not certify vendors, verifiers or SW__
- if ASVS is used for verification or "certification" reports need to be include:
	- scope of the verification
	- summary of verification findings (passes / failed tests / how to resolve failed tests)
	- documentation details (work papers, screen shots, scripts used, electronic records of testing)
#### Penetration Testing
 - use of automated pentest tools
 - ASVS cannot be automated completely (because of L2 and L3)
 - Pentesting alone not efficient