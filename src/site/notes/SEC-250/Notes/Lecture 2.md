---
{"dg-publish":true,"permalink":"/sec-250/notes/lecture-2/","dg-note-properties":{}}
---


- Information Security
	- protects sensitive data or information from unauthorized access, modification, disclosure, destruction, or disruption
	- Common goal is to protect the CIA triad
- Purpose of InfoSec
	- Protect
		- Information
		- People
		- Systems
		- Hardware used to store, transmit, and process data
		- Resources
- Consequences of inadequate InfoSec
	- Financial loss
	- reputation damage
	- data loss
	- legal liability
	- can range from inconvenient to catastrophic
- Requirements
	- The requirements of InfoSec have majorly changed over the last few decades
	- It used to be more focused on physical access
	- Automated tools are now a necessity for protecting data
- Layers of security
	- We need multiple protections at each layer
	- Communications
		- TCP/IP: since it connects everything
		- Network Security is essential to protect data that is being transmitted and guarantee the data is not tampered with during transmission
	- Physical
		- Oldest form of security
		- Can not be ignored
		- Part of any security system
		- Need to consider
			- insecure server room doors
			- physical keys and id badges
			- no timeout on systems
			- devices left open
	- Software
		- Deals with 
			- OS security
			- Application security
			- Software utilities / tools
		- Even security tools need to be secured
		- **More devices == more software == mode things that can break**
		- Security needs to be baked into every single phase of software development
		- in practice, secure design, development, and deployment is lagging behind significantly
	- People
		- Can't be fixed by fancy technology
		- takes a lot of effort to secure
		- weakest link
- Common threats right now
	- Malware
	- Ransomware
	- Phishing
	- insider threats
	- DDOS
	- SQL injection
	- Zero days
	- etc...
- The goal is not to protect against all of them, but to protect against the most critical ones for your organization.
- Security frameworks
	- ISO/IEC 27001:2013
		- Specifies management system that is intended to bring information security under management control and gives specific requirements
	- NIST Special Publication 800-39
		- Managing info security risk
	- NIST Special Publication 800-53 Revision 4
		- Security and privacy controls for federal information systems and organizations
	- SABSA
		- "methodology for developing business-driven, risk and opportunity focused security architecture, at both enterprise and solutions level that tractably support business objectives."
- None of these use the same layers but all have core layering concepts in common
- Security implementation cycle
	1. Risk Assessment
		- Why put money into it if it isn't a risk
	2. Planning & Architecture
		- Plan how you are going to deal with that risk
		- and setup the architecture
	3. Gap Analysis
		- We know what we want to do 
		- Do we already have what we need to do it?
	4. Integration & Deployment
	5. Operations
		- Create procedures and polices for managing it
	6. Monitoring & Forensic Analysis
		- No point in security controls if we don't know they are operating effectively
	7. Legal Compliance & Audit
	8. Crisis Management
	9. Back to \#1


## Slides: [Mod 2- Key Concepts and Principles.pdf](/img/user/SEC-250/Slides/Mod%202-%20Key%20Concepts%20and%20Principles.pdf)