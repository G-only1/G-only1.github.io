---
{"dg-publish":true,"permalink":"/home-lab/cisco-cme-configuration/"}
---

This is the challenges, commands, and process used to configure Cisco Call Manager Express (CME) running on a 2800 router with 7940 series IP phones.

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
## Interface Configuration
I will be using interface Fa0/0 to connect to the phones.
1. `enable`
2. `configure terminial`
3. `interface FastEthernet 0/0` enters configuration more for interface Fa0/0
4. `ip address <ip-address> <subnet-mask>` sets the interfaces IP address
5. `no shutdown` enables the interface
## DHCP Server
The DHCP server will be configured to give addresses on the 192.168.110.0 network. I used [this guide](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucme/admin/configuration/manual/cmeadm/cmenetwk.html#ucme_g_configure).

To configure the DHCP server:
1. `enable`
2. `configure terminial`
3. `ip dhcp pool <pool-name>`
4. `network <ip-address> <subnet-mask>`
5. `option 150 ip <ip-address>`
	- This tells the phones the address of the TFTP server to download configs from.
	- This should be the CME router's IP address
6. `default-router <ip-address>`