---
{"dg-publish":true,"permalink":"/sec-250/assignments/operation-shadow-breach/operation-shadow-breach/","dg-note-properties":{}}
---

#### Summary: In this lab we found a vulnerability in TechNova's system and used it to gain access to a server via SSH.
1. Tech Nova is using Apache version 2.4.49
![Pasted image 20260831123620.png](/img/user/SEC-250/Assignments/Operation%20Shadow%20Breach/_assets/Pasted%20image%2020260831123620.png)
2. CVE-2021-41773 is associated with Apache version 2.4.49
	- This is a path traversal attack that can view files and folders outside of the web root folder
	- It can also be exploited to get remote code execution on the server
	- A path traversal attack tries to access files and folders outside of the web root folder
	- https://owasp.org/www-community/attacks/Path_Traversal
3. Exploit Public-Facing Application: T1190: https://attack.mitre.org/techniques/T1190/
4. Use `nmap -sV -sC -p- 10.0.17.7` 
	- Open Ports and services running
		- 21: FTP
		- 22: SSH
		- 80: HTTP
		- 139: Samba (SMB)
		- 443: HTTPS
		- 445: Samba (SMB)
		- 3306: MySQL
		- 8080: HTTP
	- what can be used to get a remote session on the target VM?
		- SSH can be used for remote access if we can get the username and password
	- ![Pasted image 20260831125659.png\|397](/img/user/SEC-250/Assignments/Operation%20Shadow%20Breach/_assets/Pasted%20image%2020260831125659.png)
5. Find a username
	- Possible Usernames
		- Mike Chen - mchen-technova
			- DEV-OPS engineer
			- mchen@technova.local
		- Sarah Johnson - sjohnson-tech
			- sjohnson@technova.local
			- IT manager
		- Emily Watson 
			- CTO
			- ewatson@technova.local
		- Alex Rodriguez
			- arodriguez@technova.local
			- Security Analyst
			- LOGIN WORKS
		- admin
			- admin pass: TechNova2024!
6. Caesar cipher with a shift of 9
	- `Cqrwt-Lxuunlc-Yajlcrlju-Knbc-Proc`
	- decrypts to `Think-Collect-Practical-Best-Gift`
7. I was able to login to arodriguez with the above password![Pasted image 20260831131609.png](/img/user/SEC-250/Assignments/Operation%20Shadow%20Breach/_assets/Pasted%20image%2020260831131609.png)
### Reflection Questions
1. Describe the complete attack path you took from initial reconnaissance to gaining SSH access. What were the critical steps?
	1.  First we visited their website and found that they had published that they were using a vulnerable version of Apache in production, along with the version number
	2. Next we did some research and found a path traversal and remote code execution CVE that could be exploited on their Apache web-server
	3. After that we used nmap to scan the web servers IP address and find ports and services that were open
	4. Then we did some OSINT on their website to find potential usernames to login to their web server via SSH
	5. We got lucky and our friend sent us a password to the "arodriguez" account on the web-server in the form of a Caesar cipher
	6. We logged into the web server using SSH 
2. If you were TechNova's security team, at which stage do you think you could you have detected this attack? How?
	- If i was on their security team, i think it would have been difficult for us to detect this attack until it was already too late because of how much publicly available information was used. We could have detected it during the nmap scan or by monitoring the SSH access logs.
3. Rate the severity of the Apache 2.4.49 vulnerability on a scale of 1-10. Justify your rating using CVSS scores and business context.
	- I would give the Apache 2.4.49 vulnerability (CVE-2021-41773) a severity rating of 9.5 because it gives an attacker the ability to read any file on the web-server. It can also give remote code execution if CGI scripts are enabled on the server. 
4. After reading the risk acceptance form, explain WHY TechNova chose to  keep the vulnerable server running instead of patching immediately.
	- TechNova chose to keep the vulnerable server "as-is for compatibility testing" and it ended up staying like that for 3+ years. They also mentioned "Security vs. Convenience" in their LinkedIn post, they knew it was vulnerable and put off fixing it until a later date. The main reason they chose to keep the server running because of convenience.
5. Do you agree or disagree with TechNova's decision? Support your answer with specific reasoning about business vs. security trade-offs.
	- I disagree with TechNova's decision. Knowingly leaving a vulnerable server running, even if it's only a test server, should always be considered unacceptable. Even if the server is isolated from the rest of the network, mistakes happen and a misconfiguration could render the isolation useless.
6. What compensating controls (safety measures) did TechNova put in place to reduce risk while keeping the vulnerable server running?
	- TechNova put the server on an isolated subnet that was only accessible internally, and changed the SSH port to `2222` instead of port `22`.
7. Are these compensating controls adequate? What additional controls would you recommend?
	-  I don't think those compensating controls are adequate. My number one recommendation would be to **update the server** to a newer version of Apache. If they don't take that recommendation, the next best thing would be to disconnect the server from the network entirely.
8. Is it ethical for a company to knowingly run vulnerable systems?
	- I don't think its ethical for a company to knowingly run vulnerable systems because of the amount of risk involved. Any compromised server, even if its only a testing server, inside a company's network poses a risk to the entire network including all production servers and customer data.
9. Under what circumstances is this acceptable vs. negligent?
	- I think it would only be acceptable to knowingly run a vulnerable server if it was on a completely isolated network and being ran for purposes of testing the specific vulnerability in that version of the server. Any other scenerio would be neglegent in my opinion.
10. If TechNova were breached by a real attacker, should the leadership team face legal consequences? Why or why not?
	- Yes, i think TechNova's leadership should face legal consequences if they were breached by a real attacker because they knowingly left systems vulnerable. If they were unaware of the vulnerable server then it might be more excusable, but they publicly mentioned the vulnerable server multiple times and made no attempt to patch it.


## Slides: [Operation Shadow Breach - Slides.pdf](/img/user/SEC-250/Assignments/Operation%20Shadow%20Breach/Operation%20Shadow%20Breach%20-%20Slides.pdf)