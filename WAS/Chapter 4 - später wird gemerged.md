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
### C9 - Implement Security Logging and Monitoring
### C10 - Handle All Errors and Exceptions