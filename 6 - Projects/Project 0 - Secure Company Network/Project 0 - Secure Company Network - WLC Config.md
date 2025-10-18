# Project 0 - Secure Company Network - Wireless LAN Controller Configuration

## WLC Configuration Commands

**Task 12 - Configure Wireless LAN Controller**


+ From the management laptop, try to ping the WLC (10.20.0.10).
+ If successful, access the Laptop web browser (http://10.20.0.10)

+ Credentials:
	- Username: admin
	- Password: Ccompute123
	- System Name: CCOMPUTEWLC
	- Mgt IP: 10.20.0.10
	- Subnet Mask: 255.255.0.0
	- Gateway: 10.20.0.1
+ The next pages allow network admins to create SSID/WLANs.
	- SSID: STAFF
	- Passphrase: passwordstaff
+ RF Parameter optimisation settings can remain with the default settings.

+ Allow the WLC to reboot;.
+ When attempting to access the same web address, a 'Server Connection Reset' error will appear; use this address instead https://10.20.0.10.

**AP Configuration**

+ WLANs Configuration:
	- SSID: STAFF
	- Passphrase: passwordstaff
	- SSID: CORP
	- Passphrase: passwordcorp
	- SSID: EXTAUDIT
	- Passphrase: passwordaudit
	- SSID: GUEST
	- Passphrase: password

+ Go to Advanced > AP Groups (left menu):
	- Create additional groups for each AP.
		* IT / IT WiFi Users
		* HR / HR WiFi Users
		* FIN / FIN WiFi Users
		* GUEST / GUEST WiFi Users
	- For each AP Group.
		* WLANs & APs (tabs) > Add its respective WLAN and AP.