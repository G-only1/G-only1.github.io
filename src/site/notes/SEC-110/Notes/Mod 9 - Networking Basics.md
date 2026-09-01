---
{"dg-publish":true,"permalink":"/sec-110/notes/mod-9-networking-basics/","tags":["SEC-110","Notes"],"dg-note-properties":{"date":"2026-03-25","Instructor":"LaKysha Patnode","Lecture Slides":"[[Mod 9 - Networking Basics.pdf]]","Module #":["9"],"tags":["SEC-110","Notes"]}}
---

# Notes
- **Network Topologies**
	- Bus
		- Devices are connected via a single cable
		- Simple and cost effective, but insecure and may have performance issues
	- Ring
		- Devices are connected on a loop where data travels one direction
		- Simple, but not scalable
		- Reduced data collisions, but still prone to performance issues
	- Star
		- Devices all connect to a central hub
		- Scalable, simple troubleshooting
		- Central device is a point of failure
	- Mesh
		- Devices each connect to multiple other devices, creating a web or “mesh”
		- Expensive + complex setup, not scalable
		- Very reliable, fault tolerant, good bandwidth
- A hybrid star / mesh topology is most common today
- **Network Devices**
	- Servers: provide resources, data, programs, or services to other devices
	- Hubs: a simple network device that sends incoming data to all attached devices
	- Switches: connects multiple devices and direct packets to their destination device
	- Routers: connect networks to each other and direct packets to their destination network
	- Bridges: connect network segments to make them act as a singular network
	- Access Points: allow wireless devices to connect to a wired network
- **Types of Networks**
	- Networks are categorized based on geographic range, number of devices, and how they connect
	- *LAN* (Local Area Network): covers a small area such as a home or school
	- *WLAN* (Wireless Local Area Network): LAN that uses WiFi instead of cables
	- *WAN* (Wide Area Network): covers large geographical areas such as cities or countries, connecting multiple LANs
	- *MAN* (Metropolitan Area Network): covers a city or small campus (in-between LAN and WAN)
	- *PAN* (Personal Area Network): a very small network consisting of connections between one person’s devices
- **Network Addressing**
	- IP Address
		- Parts of an IP Address
			- Network ID
				- Beginning of address
				- Anywhere from 8 to 30 bits
			- Host ID
				- Whatever is left over from Net ID
			- Subnet Mask: 
				- Defines how many bits are in the Net ID
		- IP Address Example
			- Binary IP address: 
				- 10000001101010100001001011011100
			- Translated to dotted decimal: 
				- 129.170.18.220
			- Subnet Mask is /24 
				- First 24 bits are Network ID
			- Net ID: 129.170.18 
			- Host ID: 220
	- MAC Address
		- Unique to every network adapter
- **Domain Name System (DNS)**
	- A hierarchical, distributed system that translates website names into IP addresses, like a phonebook for the internet.
	- Example: 
		- my.champlain.edu is 216.93.150.217 
		- google.com is 8.8.8.8
- **Open Systems Interconnection (OSI) Model**
	- Network protocol framework that uses 7 layers
	- The layers define the different stages that data goes through to travel from one device to another over a network 
	- *OSI Model Layers:*
		1. Physical Layer (1)
			- Wires and signals 
			- carries data
		2. Data Link Layer (2) 
			- Getting data across the wires 
			- MAC addresses
		3. Network Layer (3) 
			- Finding the best route 
			- IP addresses and routers
		4. Transport Layer (4) 
			- Keeping data in order 
			- TCP and UDP
		5. Session Layer (5)
			- Managing connections
			- Keeping you connected
		6. Presentation Layer (6) 
			- Translation Data
			- convert data into a format your device understands 
		7. Application Layer (7) 
			- What you see and use
- **Network Security Toolkit: Wireshark**
	- A popular tool for “capturing” network traffic. (also called sniffing
	- Captures all traffic to and from local host
	- Will parse packet “headers” and display the info in an organized way
	- Can filter results to show specific sessions, protocols, hosts, etc.
## Vocabulary & Key Terms
### Network 
- A network is a group of interconnected devices that communicate and exchange data.
- Networks can have different topologies, devices, and sizes/types.
___

# Slides
[[SEC-110/Slides/Mod 9 - Networking Basics (slides)\|Mod 9 - Networking Basics (slides)]]