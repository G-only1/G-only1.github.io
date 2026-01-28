---
{"dg-publish":true,"permalink":"/sec-110/notes/mod-2-intro-to-kali/","tags":["SEC-110","Notes"]}
---

# Notes
- We will be using Kali Linux in this class
	- Kali is Debian based
- Common Scripting languages
	- Python
		- Very versatile, many libraries
	- Bash 
		- We will be using Bash most of the time in this class.
		- Native to UNIX / Linux
		- Stands for *Bourne Again Shell*
	- JavaScript
		- Mostly used for web pages
		- Works well with HTML and CSS
	- PowerShell
		- Built on .NET framework
		- Object oriented
## Vocabulary & Key Terms
___
### Virtual Machine (VM)
A virtual operating system running on top of a hypervisor
___
### Hypervisor
A Hypervisor is software that allows a host machine to create and run VMs
- Proxmox
- VMWare
- VirtuialBox
___
### Operating System (OS)
An Operating System manages:
- Memory
- Processes
- All software
- All hardware
Common Operating Systems are:
- Microsoft Windows
- macOS
- Linux
- IOS
- Android
- ChromeOS
## Commands, Tools, or Techniques
___
### Useful Commands
- **pwd** (Prints working directory)
- **mkdir** ( Creates a directory within the current directory)
- **cd** (changes the current working directory)
- **ls** (Displays all files and directories in the current directory)
- **ls -l** (Lists files & file permissions)
- **clear** (Clears text from the terminal)
- **touch** (Creates a file without entering a text editor)
- **mv** (Moves a file or directory)
- **sudo** (runs command with administrator privileges after authentication)
- **chmod** (Changes permissions on files)

___
### Logging into the Cyber Range
1. Go to https://range.leahycenter.org
2. Enter your Champlain email to be sent a one time code
3. Enter your proxmox username and password.
4. Use the realm `outreach.local`
5. Login
___
### Adding a new user
1. use `sudo su` to become superuser
2. type `adduser <user_name>` to create a user
3. Set the password (the `adduser` command will ask for the password)
4. Add the user to the sudo group using `usermod -aG sudo <user_name>`
___
## Useful Resources
- https://www.kali.org/tools/