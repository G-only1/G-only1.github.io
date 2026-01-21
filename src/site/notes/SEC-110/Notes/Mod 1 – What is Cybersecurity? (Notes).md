---
{"dg-publish":true,"permalink":"/sec-110/notes/mod-1-what-is-cybersecurity-notes/","tags":["SEC-110","Notes"]}
---

# Notes
- Cybersecurity is fundamentally defensive.
- To predict threats & defend attacks you need to think like an attacker
- Systems designed to trust users can be exploited when opened to the public
- Hackers like to go for easy targets
- Humans are often the weakest link 
## Vocabulary & Key Terms
---
### Cybersecurity
Cybersecurity is the method of defending computer systems, networks, and data from illegal access, harm, or utilization.
- Methods 
- Processes 
- Tools 
- Behaviors 
- Policies

Why it matters:
- Critical infrastructure is becoming increasingly digital
- Data is valuable, it needs protection
- Personal information could be vulnerable
- Cyber-attacks can cost millions
- Necessary to comply with regulations (GDPR, HIPAA, and PCI-DSS)
---
### CIA Triad
The CIA triad is the main base of cybersecurity

- Confidentiality: protecting personal information from unauthorized access, securing only authorized persons can access sensitive data. 
	- Data can be used to hurt people & organizations
	- Data Classification
	- GDPR, HIPAA
	- 
- Integrity: securing information is correct, complete, and hasn’t be changed without authorization. 
	- You should be able to trust your data
	- Checksums
	- Version Control
	- Access Logs
- Availability: securing that authorized individuals can access data and resources when needed. 
	- Backup systems
	- Redundant systems
	- DDOS protections
	- Disaster recovery planning

The CIA triad is a balance. Some parts can negate others.

---
### Zero Trust
Dont trust anyone, not even yourself. Always check, and consistently check. Assumes no device should be trusted by default
- Users only receive access to resources they need
- Always verify
- Design network assuming attackers are already inside our network

Initially networks used trust but verify
___
### Authentication: Who are you?
Authentication is the process of verifying someones identity
#### Methods:
- Something you know
	- Password
- Something you have
	- Code from app
	- USB token
- Something you are (Biometrics)
	- Fingerprint
	- Face scan

Multi-Factor authentication should be required to authenticate.
___
### Authorization: What can you do:
Authorization determines what resources or actions a person is allowed to access
#### Types
- Role-based Access Control (RBAC):
	Access based on job role
-  Attribute-based Access Control (ABAC): 
	Access based on the person’s attributes and conditions 
___
### Defense In Depth
A multi-layered approach to security controls
- Ensures that if one security measure fails, there ate others in place behind it
- Minimizes likelihood of breach
- Gives time to launch countermeasures against an attack
	- Ideally Network monitoring will be in place
___
## Main Ideas and Takeaways
---
### Human Factors
Humans are the weakest link
- between 68% to 95% of cyber incidents are caused by human error
- Social engineering attacks
	- Phishing
	- Pretexting
	- Baiting
- Lack of awareness & training
	- Not knowing what to do when a problem occurs
- Bad password hygiene or authentication
	- Weak passwords
- Bad communication
	- User not reporting something suspicious 
	- Users should feel safe to report suspicious activity
___
### The Cybersecurity Mindset
Having a cybersecurity mindset is about more than stopping attackers
- Prepared, alert, and ethical
- Aware of their online presence and digital footprint
- **Always learning** new technology, tools, or exploits
	- Read security blogs
	- Keep up to date with tech news
	- Practicing labs
___
# Slides
<iframe src="/img/user/SEC-110/Slides/Module%201%20What%20is%20Cybersecurity.pdf" width="100%" height="900px" title="Module 1 What is Cybersecurity.pdf" style="border:1px solid #ccc;"></iframe>