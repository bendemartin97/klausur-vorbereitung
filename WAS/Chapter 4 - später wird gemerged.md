
### C1 - Define Security Requirements 
- security requirements: __statement of needed security functionality__
	- derived from industry standards
	- define new features to solve specific security problem or elimante vulnerability
	- provide a foundation for security
- standard security requirements for reusing 
	- prevent the repeat of past security failures
- Application Security Verifactiion Standard 4.0.1:
	- first application security standard 
	- defines 3 risk levels with 280 controls
	- _3 Levels:_
		1. Baseline: min. required for all app, mostly fully testable and automatable
		2. Standard: suitable for sensitive data, 75% testable, somewhat automatable
		3. Comprehensive: suitable for critical app, mostly testabla via automation, manual verifacitions required
- includes the writing of unit tests
	- validate your app each and every build
	- allows to concentrate on difficult to automate test (business logic flaws, access control issues etc.)
### C2 - Leverage Security Frameworks and Libraries
- help guard against security-related desing and implementation flaws
- because of sufficient knowledge, time, or budget no 
### C3 - Secure Database Access  
### C4 - Encode and Escape Data  
### C5 - Validate All Inputs  
### C6 - Implement Digital Identity  
### C7 - Enforce Access Controls  
### C8 - Protect Data Everywhere  
### C9 - Implement Security Logging and Monitoring
### C10 - Handle All Errors and Exceptions