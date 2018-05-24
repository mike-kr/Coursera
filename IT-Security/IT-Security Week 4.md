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