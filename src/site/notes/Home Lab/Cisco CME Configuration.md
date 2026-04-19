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
## Interface Configuration
## DHCP Server
