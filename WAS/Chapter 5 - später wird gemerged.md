### Application Security Verification Standard
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
- applications achives, if it adequately defends against advanced application security vulnerabilities and also demonstrates principles of good security design
- for applications with
	- required significant security level
	- perform ciritical functions
- demostrate a good security design ( modularized, each module seperated by network etc.)
### Applying ASVS in Practice
- not certify vendors, verifiers or SW
- if ASVS is used for verification or "certification" reports need to be include:
	- scope of the verification
	- summary of verification findings (passes / failed tests / how to resolve failed tests)
	- documentation details (work papers, screen shots, scripts used, electronic records of testing)