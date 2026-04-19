---
{"dg-publish":true,"permalink":"/home-lab/cisco-cme-configuration/"}
---

This is the challenges, commands, and process used to configure Cisco Call Manager Express (CME) running on a 2800 router with 7940 series IP phones.

# Switch Configuration
My switch is a 24 port Cisco 3500XL with inline power (POE before POE). It is running a very old version of IOS (12.4) so some commands might be different on newer versions.
## Trunk Port to Connect to CME Router
The trunk port will be used to connect to port Fa0/0 on the router running CME
1. `enable`
2. `configure terminial`
3. `interface FastEthernet 0/1`
4. `description CONNECTION TO ROUTER-ON-A-STICK CME ROUTER`
5. `switchport trunk encapsulation dot1q`
6. `switchport mode trunk`
7. `switchport trunk native vlan 110`
8. `end`
9. `write`
## VLAN Configuration
1. `enable`
2. `vlan database`
3. `vlan 115`
4. `vlan 115 name VOICE`
5. `vlan 110`
6. `vlan 110 name DATA`
7. `apply`
8. `exit`
9. `write`
## Assigning Switchport to a VLAN
1. `enable`
2. `configure terminial`
3. `interface fastEthernet 0/2`
4. `switchport mode access`
5. `switchport access vlan 110`
6. `switchport voice vlan 115`
> [!note]
> To configure multiple switchports at once use `interface range <interface>`

# Router Configuration
I have a Cisco 2800 router running IOS version 12.4
## Resetting Router Password
Its been a few years since I've used this router and i don't remember the password. I roughly followed this [guide to reset the password](https://www.cisco.com/c/en/us/support/docs/routers/3800-series-integrated-services-routers/112058-c1900-pwd-rec-00.html). These steps can also be used to factory reset the router if you do the opposite of what the warning says at step 8.
1. Turn off the router with the power switch
2. Remove the CF card and power on the router
3. The router will boot into ROMMON mode
4. Type `confreg 0x2142` at the `rommon 1>` prompt
	- This will bypass the startup config where the passwords are stored
5. Insert the CF card and type `boot` at the `rommon 2>` prompt
	- The router will boot and ignore the saved config
6. Type `no` after each setup question, or press `Ctrl-C` to skip the initial setup procedure
7. Type `enable` at the Router> prompt
8. Type `configure memory` or `copy startup-config running-config` in order to copy the nonvolatile RAM (NVRAM) into memory
	- > [!danger]
	  > Do **not** enter `copy running-config startup-config` or `write` at this point. These commands erase your startup configuration
9. Type `configure terminal`
10. Type `enable secret <password>` to change the enable secret password
11. Issue the `no shutdown` command on every interface that you use
12. Type `config-register 0x2102`
13. Press **Ctrl-z** or **end** in order to leave the configuration mode
14. Type `write memory` or `copy running-config startup-config` in order to commit the changes
## Disabling annoying log messages
One of the fans on my router is broken, which causes an error message to be sent to the console roughly every 15 seconds. This gets **very** annoying after a while.

To disable logging errors to console:
1. Type `configure terminial` to enter global configuration mode
2. Type `no logging console` to disable logging to console
3. Type `exit` to leave global configuration mode
4. Type `write memory` to save the configuration
## Setting the correct time
Make sure you use 24 hour time (military time) when setting the clock
1. `enable`
2. `clock set <hh:mm:ss> <day> <month> <year>`
	- In my case the command was: `clock set 23:44:00 18 April 2026`
3. `write`
## Interface & VLAN Configuration
I will be using interface Fa0/0 to connect to the phones. VLAN 115 will be the voice VLAN and 110 will be the data VLAN.
1. `enable`
2. `configure terminial`
3. `interface FastEthernet 0/0` enters configuration more for interface Fa0/0
4. `no ip address` remove IP address from interface
5. `interface fa0/0.115` enters configuration more for interface Fa0/0.115
6. `description ROUTER INTERFACE VOICE VLAN` add description to interface
7. `encapsulation dot1q 115`
8. `ip address 192.168.115.1 255.255.255.0` sets the interfaces IP address
9. `exit`
10. `interface fa0/0.110` enters configuration more for interface Fa0/0.110
11. `description ROUTER INTERFACE FOR DATA VLAN` add description to interface
12. `encapsulation dot1q 110 native`
13. `ip address 192.168.110.1 255.255.255.0`
14. `end`
15. `write`
## DHCP Server Configuration
The DHCP server will be configured to give addresses on the 192.168.110.0 network. I used [this guide](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucme/admin/configuration/manual/cmeadm/cmenetwk.html#ucme_g_configure).
To configure the DHCP server:
1. `enable`
2. `configure terminial`
3. `ip dhcp excluded-address 192.168.115.1 192.168.115.10`
4. `ip dhcp excluded-address 192.168.110.1 192.168.110.10`
5. `ip dhcp pool VOICE-POOL`
6. `network 192.168.115.0 255.255.255.0`
7. `default-router 192.168.115.1`
8. `option 150 ip <ip-address>`
	- This tells the phones the address of the TFTP server to download configs from.
	- This should be the CME router's IP address
9. `dns-server 192.168.115.1`
10. `exit`
11. `ip dhcp pool DATA-POOL`
12. `network 192.168.110.0 255.255.255.0`
13. `default-router 192.168.110.1`
14. `dns-server 192.168.110.1`
15. `end`
16. `write`
## TFTP Server Configuration
The TFTP server is how the phones will get their configurations as well as ringtones or any other files they need. The specific file names and paths can vary depending on the version you have, make sure to double check they are correct. The phones are very picky about it.
1. `enable`
2. `configure terminial`
3. `tftp-server flash:7940-7960/8-1-1/P00308010100.bin`
4. `tftp-server flash:7940-7960/8-1-1/P00308010100.loads`
5. `tftp-server flash:7940-7960/8-1-1/P00308010100.sb2`
6. `tftp-server flash:7940-7960/8-1-1/P00308010100.sbn`
7. `end`
8. `write`
## Telephony Service Configuration
Finally getting to the good part!
1. `enable`
2. `configure terminial`
3. `telephony-service`
4. `max-ephones 10`
5. `max-dn 20`
6. `ip source address 192.168.115.1`

# Resources
[how to configure cme.pdf](https://mrncciew.com/wp-content/uploads/2013/07/how-to-configure-cme.pdf)
[Cisco Unified Communications Manager Express System Administrator Guide](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucme/admin/configuration/manual/cmeadm/cmenetwk.html)
[Configuring VLANs on Cisco Switches](https://www.practicalnetworking.net/stand-alone/configuring-vlans/)