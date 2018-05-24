# Secure Network Architecture
## Network Hardening Best Practices
**Network hardening** - the process of securing a network by reducing its potential vulnerabilities through configuration changes and taking specific steps

**Implicit Deny** - a network security concept where anything not explicitly permitted or allowed should be denied
* Whitelisting opposed to blacklisting

Monitor and analyze your traffic
* establish a baseline of your typical network traffic

**Analyzing Logs** - the practice of collecting logs from different network and sometimes client devices on your network, then performing an automated analysis on them

**Logs analysis systems** are configured using user-defined rules to match interesting or atypical log entries.

**Normalizing log data** is an important step, since logs from different devices and systems may not be formatted in a common way.

**Correlation analysis** - the process of taking log data from different systems and matching events across the systems

**Post-fail analysis** - investigates how a breach happened after it has been detected

* Splunk
* Graylog

**Flood guards** - provide protection against DoS or Denial of Service attacks
* fail2ban

**Network Separation (or Network Segmentation)** -  use vlans to create virtual networks for different device classes or types

[Cisco IOS firewall rules](https://www.cisco.com/c/en/us/td/docs/security/security_management/cisco_security_manager/security_manager/4-1/user/guide/CSMUserGuide_wrapper/fwaccess.html)  
[Juniper firewall rules](https://www.juniper.net/documentation/en_US/junos/topics/usage-guidelines/services-configuring-stateful-firewall-rules.html)  
[Iptables firewall rules](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands)  
[UFW firewall rules](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)  
[Configuring Mac OS X firewall](https://support.apple.com/en-us/HT201642)  
[Microsoft firewall rules](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754274(v=ws.11))

## Network Hardware Hardening
**Rogue DHCP server attack** - protect with **DHCP snooping**
* also protects against IP spoofing and ARP poisoning attacks
* must designate a trusted DHCP server IP if it's operating as a DHCP helper, or you can enable DHCP snooping trust on the uplinked port

**Dynamic ARP inspection (DAI)** - gratuitous arp response - MITM attack
* uses DHCP snooping to establish a trusted binding of IP addresses to switch ports
* enforcing rate limiting of ARP packets per port to prevent ARP scanning

**IP Source Guard (IPSG)** - IP spoofing attacks
* uses DHCP snooping tables to create ACLs for each switch port

**802.1x** - IEEE standard for encapsulating EAP or Extensible Authentication Protocol traffic over the 802 networks
* also called EAP over LAN or EAPOL
* Client is called supplicant (wpa_supplicant) that communicates with authenticator (requires clients to successfully authenticate with the network before they are allowed to communicate with the network. This is usually an enterprise switch or access point)
* authenticator doesn't actually authenticate, it just forwards the request to the authentication server (usually a RADIUS server)

**EAP-TLS** - an authentication type supported by EAP that uses TLS to provide mutual authentication of both the client and the authenticating server
* most implementations require client-side certificates
* to secure certificates, bind the certificates to TPMs combined with FDE

## Network Software Hardening
* firewalls
* proxies
* vpns

**Firewalls**
Can be dedicated network infrastructure or host-based as software  
Generally recommended to utilize both solutions  
Host-based firewalls also help protect against compromised devices on the internal network

**VPNs**  
Commonly used to provide **secure remote access**, and **link two networks** securely

**proxies**  
protect client devices and their traffic as well as provide secure remote access without using a VPN  
(connect to proxy server)

**reverse proxy** can be configured to allow secure remote access to web based services without requiring a VPN  
incoming requests are intercepted by proxy and forwarded
* HAproxy
* Nginx
* Apache web server
# Wireless Security
## WEP Encryption and Why You Shouldn't Use It
**WEP (Wired Equivalent Privacy)** - Never use WEP!
* used RC4 symmetric cipher for encryption using either 40-bit or 104-bit shared key
* created by combining user-supplied shared key and joining a 24-bit IV (initialization vector)
* together this means a 64-bit key or 128-bit key is used
* 2 modes
  * Open System Authentication
  * Shared Key Authentication
* Since shared key doesn't change frequently, there is only a 24-bit pool (from IV) so roughly every 5000 packets the IV is reused thus the encryption key is also reused (the IV is also just transmitted in plain text)
* Aircrack-ng
* AirSnort
## Let's Get Rid of WEP! WPA/WPA2
**WPA (WiFi Protected Access)** - designed as a short-term replacement that would be compatible with older WEP-enabled hardware with a simple firmware update

**TKIP (Temporal Key Integrity Protocol)**
1. a more secure key derivation method was used to more securely incorporate the IV into the per packet encryption key
2. a sequence counter was implemented to prevent replay attacks by rejecting out of order packets
3. a 64-bit MIC or Message Integrity Check was introduced to prevent forging, tampering, or corruption of packets
* Still uses RC4 cipher with key mixing function and 256-bit long keys
* Under WPA, the **pre-shared key** is the Wifi password you share with people when they come over and want to use your wireless network
  * isn't used to directly encrypt traffic
  * passphrase is fed into the **PBKDF2** or Password-Based Key Derivation Function 2 along with the Wi-Fi networks SSID as a salt. This is then run through the **HMAC-SHA1** function 4096 times to generate a unique encryption key.
  * The SSID salt is incorporated to help defend against rainbow table attacks

WPA2 improved WPA even more by introducing **CCMP** or counter mode CBC-MAC protocol
* based on AES cipher
* pre-shared requirements are the same as well as key derivation procedure

CBC-MAC - is a particular mode of operation for block ciphers
* authenticated encryption

**4-way handshake** authenticates clients to the network
Designed so an AP can ensure a client has the correct **PMK** pairwise master key, or pre-shared key in a WPA-PSK setup without disclosing the PMK.  The PMK is a long lived key and might not change for a long time so an encryption key is derived from the PMK that's used for actual encryption and decryption of traffic between a client and AP.  This is called the **Pairwise Transient Key or PTK**

The PTK is generated using the:
* PMK
* AP nonce (random bits of data generated by each party and exchanged)
* Client nonce (random bits of data generated by each party and exchanged)
* AP MAC address
* Client MAC address

PTK is made up of 5 individual keys:  
2 keys are used for encryption and confirmation of EAPoL packets, and the encapsulating protocol carries these messages  
2 keys are used for sending and receiving message integrity codes  
1 temporal key, which is actually used to encrypt data

AP will also transmit the **GTK** Groupwise Transient Key (Encrypted using the EAPoL key contained in the PTK used to encrypt multicast or broadcast traffic)

GTK is shared between all clients and retransmitted periodically and when a client disassociates the AP

STEPS:
1. AP sends a nonce to the client
2. Client sends its nonce to the AP + MIC
3. AP sends the GTK + MIC
4. Client resonds with an ACK

WPA2-Enterprise 802.1x

non 802.1x configurations are called:
* WPA2-Personal
* WPA2-PSK

They use a pre-shared key to authenticate

**WPS**  
devised way to securely join a wireless network without having to directly enter the pre-shared key

* PIN entry authentication
  * client generates a pin or
  * ap has hardcoded pin
    * vulnerable to brute force attacks
  * pins are 8 digits long but the last digit is a checksum
  * client sends first 4 digits then waits for positive or negative response then sends the last 4
  * lockout period of 1 minute after 3 incorrect pin attempts
* NFC or USB
* Push-button authentication

Attacker can capture 4 way handshake packets and then attack using brute force offline

## Wireless Hardening
Ideal - 802.1x with EAP-TLS

if 802.1x is too complicated for a company, the next best alternative would be WPA2 with AES/CCMP mode  
A long and complex passphrase that wouldn't be found in a dictionary would increase the amount of time and resources an attacker would need to break the passphrase  
Also change the SSID to something uncommon and unique

If your company values security over convenience, you should make sure that WPS isn't enabled on your APs
* Wash - tool that scans and enumerates aps that have WPS enabled

# Network Monitoring

 
