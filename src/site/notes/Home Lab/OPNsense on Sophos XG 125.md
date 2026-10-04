---
{"dg-publish":true,"permalink":"/home-lab/op-nsense-on-sophos-xg-125/","dg-note-properties":{}}
---

I got a Sophos XG 125 that will be replacing my current firewall and router. I'll be installing OPNsense on it instead of Sophos's router OS. 

# Installation
To install OPNsense i simply had to download the [latest ISO installer](https://opnsense.org/download/), flash it to a USB drive, boot it up on the firewall, and follow the OPNsense [installation instructions](https://docs.opnsense.org/manual/install.html). I used the serial image type and connected to the router with PuTTY at 115200 baud.
# Configuration
## Assigning Interfaces
By default none of the interfaces are assigned, I used the assign interfaces option in the serial console to map each physical port to an interface in OPNsense. I used the auto-detect feature because I didn't know which port went to which port name. 

This is what I ended up with.
![Sophos-XG-125-Ports.drawio(2) 1.png](/img/user/Home%20Lab/_assets/Sophos-XG-125-Ports.drawio(2)%201.png)
Later i removed OPT3 and put ix0 and igb0 into a LAGG.
## Accessing The Web Interface
The default LAN IP address after OPNsense is installed is `192.168.1.1`. 

Initially i couldn't ping the router or access the web interface at all. After some troubleshooting i saw that OPNsense had configured both the LAN and OPT3 interfaces with the same 192.168.1.1 IP address. First i tried removing the IP address from OPT3 via the serial console but nothing was changed, I ended up changing the IP address of OPT3 to something else that wasn't in use. After that I could access the router over the network just fine.
## Initial Configuration
After getting into the web UI it dumped me straight into a setup wizard. I just went through it and filled out the info i needed.
- General Information
	- Hostname: RT-01
	- Domain: corp.strand.systems
	- Timezone: America/New_York
	- DNS Servers: 1.1.1.1, 8.8.8.8
	- Override DNS: Unchecked
- Network \[WAN]
	- Kept default settings
- Network \[LAN]
	- Kept default settings
- Deployment Type
	- Optimize for Multiwan: Unchecked
	- Automatic DHCP/DNS registration: Checked
	- Optimize for IPsec: Unchecked

## OPT1 Anti Lockout
This is to make recovery easier if I mess up the LAN interface or somehow loose network access. 

1. Assigned IP address `192.168.2.1/24` to OPT1 via the web interface
2. Enable DHCP by adding a new DHCP range to Dnsmasq DNS & DHCP.
![Pasted image 20260930234303.png](/img/user/Home%20Lab/_assets/Pasted%20image%2020260930234303.png)
3. Add OPT1 as an interface under: Services > Dnsmasq DNS & DHCP > General
	- Every interface you want to use a DHCP server on needs to be added as an interface.
![Pasted image 20261001000840.png](/img/user/Home%20Lab/_assets/Pasted%20image%2020261001000840.png)
4. Add a firewall rule to allow all traffic from OPT1 to ALL other interfaces
![Pasted image 20260930234154.png](/img/user/Home%20Lab/_assets/Pasted%20image%2020260930234154.png)
After that you should be able to access the router through OPT1 and 192.168.2.1 as well as LAN.
## LAGG Configuration
I want to have a LAGG setup between the switch and router for redundancy and load balancing. I'm going to use ports `ix0` and `igb0` for this.

1. Remove OPT3  to free up an interface to create the LAGG
2. Create the LAGG by going to Devices > LAGG
![Pasted image 20260930234858.png\|374](/img/user/Home%20Lab/_assets/Pasted%20image%2020260930234858.png)
3. Edit LANs assignment to change the device LAN uses to the new LAGG.
![Pasted image 20260930234950.png\|458](/img/user/Home%20Lab/_assets/Pasted%20image%2020260930234950.png)
4. Add the device LAN used to be using to the LAGG

In order for LAN connection to work, the switch ports on the other side of the connection need to be properly configured to match the LAGG config on the router.

## VLAN Config
This is how I laid out my VLANs:

| VLAN ID | Subnet             | DHCP Range      | Name        |
| ------- | ------------------ | --------------- | ----------- |
| ~~1~~   | ~~192.168.1.0/24~~ | ~~.15 to .254~~ | ~~DEFAULT~~ |
| 3       | 192.168.3.0/24     | .50 to .254     | DMZ         |
| 5       | 192.168.5.0/24     | .100 to .254    | MGMT        |
| 7       | 192.168.7.0/24     | .100 to .254    | LAB         |
| 10      | 192.168.10.0/24    | .15 to .254     | LAN         |
| 12      | 192.168.12.0/24    | .50 to .254     | Cameras     |
| 14      | 192.168.14.0/24    | .15 to .254     | Guest       |
| 15      | 192.168.15.0/24    | .15 to .254     | WLAN        |
| 17      | 192.168.17.0/24    | .15 to .254     | IoT         |
I added each VLAN in `Interfaces > Devices > VLAN`
![Pasted image 20261001091155.png](/img/user/Home%20Lab/_assets/Pasted%20image%2020261001091155.png)
![Pasted image 20261001091732.png](/img/user/Home%20Lab/_assets/Pasted%20image%2020261001091732.png)
After adding the VLANs they need to be assigned in `Interfaces > Assignments`. I assigned every VLAN except VLAN 10 because I set VLAN 10 as the device used for the LAN interface.
![Pasted image 20261001093328.png](/img/user/Home%20Lab/_assets/Pasted%20image%2020261001093328.png)![Pasted image 20261001093350.png](/img/user/Home%20Lab/_assets/Pasted%20image%2020261001093350.png)
Then i went into each new interface and enabled it. 
![Pasted image 20261001092539.png](/img/user/Home%20Lab/_assets/Pasted%20image%2020261001092539.png)
After enabling an interface, i assigned it an IP address.

1. Add each VLAN in `Interfaces > Devices > VLAN`
2. Assign each VLAN to an interface in `Interfaces > Assignments`
	- I assigned VLAN 10 to LAN, all other VLANs got their own new assignment.
3. Enable each new interface
4. Assign an IP address to each new interface
5. Configure DHCP pools for each subnet in `Services > Dnsmasq DNS & DHCP > DHCP ranges`
	1. Make sure to select every interface you added a DHCP pool for in `Services > Dnsmasq DNS & DHCP > General > Interface`. DHCP will not work if you don't do this.
## VPN Config
To do later. Eventually I am going to configure a [[Site to Site VPN\|Site to Site VPN]] between the house and the cabin. 
## Firewall Rules
I'm setting up my firewall with three groups called TRUST, WIFI, and UNTRUST. Each group will be able to access the groups above it but not the groups below it.
1. Add groups and assign ports to each group
![Pasted image 20261001095209.png](/img/user/Home%20Lab/_assets/Pasted%20image%2020261001095209.png)
2. Add firewall to allow TRUST to WIFI and UNTRUST
3. Add firewall to allow WIFI to UNTRUST
![Pasted image 20261003203618.png](/img/user/Home%20Lab/_asstes/Pasted%20image%2020261003203618.png)
## Port Forwarding
I am going to copy over my existing port forwarding rules from my current router.
![Pasted image 20261003205623.png](/img/user/Home%20Lab/_asstes/Pasted%20image%2020261003205623.png)
## Dynamic DNS Config
I did a combination of following the [OPNsense documentation](https://docs.opnsense.org/manual/dynamic_dns.html) and copying settings from my old installation.
1. Go to `System > Firmware > Plugins` , search for "**os-ddclient**", and install it
2. Go to `Services > Dynamic DNS > Settings > General settings`
3. Set **Backend** to "ddclient"
4. Go to `Services > Dynamic DNS > Settings > Accounts`
5. Copied settings from old installation to the new installation
6. Don't forget to apply changes.
## DNS Config
By default, OPNsense uses Unbound DNS server. I only added a block lists for ads and privacy in `Unbound DNS > Blocklists`. Everything else was left the same

## Routing Config
To do later when the router for the cabin is configured
## Intrusion Detection
1. Enable Intrusion detection in `Services > Intrusion Detection > Administration`
2. Set to `PCAP live mode (IDS)`
3. Enable Promiscuous mode
4. Enable and download Rulesets in the Download tab
## Monitoring
To Do later.