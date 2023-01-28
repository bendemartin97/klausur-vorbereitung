### Same Origin Policy
- web browser permits scripts contained in a first web page to access data in a second web page, but _only if both web pages have the same origin_ (same URI scheme, host and port)

### Cross-orogin Resource Sharing
- _allows restricted resources on a web page to be requested from another domain_ outside the domain from which the first resource was served

### A01 - Broken Access Control
- moving up from the fifth position
- Access control enforces policy such that **users cannot act outside of their intended permissions**
- Common vulneralibities:
	- if principli of least privilege is violated, or a user can do something, for wich he does not have any permission
	- violation of principle of least privilege
	- bypassing access control checks : modifying url, api request, html page
	- permitting vieweing or editing someone else's account
	- accessing api with missing access controls for post, put, delete
	- elevation of privilege: acting as an admin, as user or acting as user without being logged in
	- metadata manipulation: cookie or jwt
	- cors misconfiguration: api access from untrusted origins
	- force browsing: to authenticated pages as an unauthenticated user