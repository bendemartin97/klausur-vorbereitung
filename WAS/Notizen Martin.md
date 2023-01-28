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

### A02 Cryptographic Failures
- moving from the third position