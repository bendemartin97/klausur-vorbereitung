
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
### C4 - Encode and Escape Data  
### C5 - Validate All Inputs  
### C6 - Implement Digital Identity  
### C7 - Enforce Access Controls  
### C8 - Protect Data Everywhere  
### C9 - Implement Security Logging and Monitoring
### C10 - Handle All Errors and Exceptions