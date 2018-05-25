# System Hardening
## Intro to Defense in Depth
**Defense in depth** - concept of having multiple, overlapping systems of defense to protect IT systems
## Disabling Unnecessary Components
* **Attack Vectors** - the method or mechanism by which an attacker or malware gains access to a network or system
  * email attachments
  * network protocols or services
  * network interfaces
  * user input
* **Attack Surfaces** - the sum of all the different attack vectors in a given system

Keep attack surfaces as small as possible  
the less complex something is, the less likely there will be undetected flaws  
Disable extra services / protocols  
Only allow access when totally necessary (ACLs)  
Another way to keep things simple, it to reduce your software deployments  
Be sure to disable unnecessary or unused components of software and systems deployed  

Telnet access for a managed switch has no business being enabled in a real-world environment

Vendor-specific API access should also be disabled if you don't plan on using these services or tools

## Host-based Firewall
**Host-based firewalls** - protect individual host from being compromised when they're used in untrusted, potentially malicious environments.

A **host-based firewall** plays a big part in reducing what's accessible to an outside attacker
* provides flexibility while only permitting connections to selective services on a given host from specific networks or IP ranges

Bastion hosts or networks - ability to restrict connections from certain origins usually used to implement a highly secure host to network.  Access to critical or sensitive systems or infrastructure is permitted

If the users of the system have administrator rights, then they have the ability to **change firewall rules and configurations**

Disable ability to disable host-based firewalls

## Logging and Auditing
Security information and even management systems **SIEMS**
* centralized logging for security information purposes

Normalization - taking log data in different formats and converting it into a standardized format

Once logs are centralized and standardized, you can write automated alerting based on **rules**

Retention

SIEMS:
* [rsyslog](https://github.com/rsyslog/rsyslog)
* [Splunk Enterprise Security](https://www.splunk.com/)
* [IBM Security Qradar](https://www.ibm.com/security/security-intelligence/qradar)
* [RSA Security Analytics](https://community.rsa.com/docs/DOC-41639)

## Anti-malware Protection
Lots of unprotected systems would be compromised **in a matter of minutes** if directly connected to the internet without any safeguards or protections in place

Anti-virus software - signature based
* Anti-virus software will monitor and analyze things, like new files being created or being modified on the system, in order to watch for any behavior that matches a known malware signature

cons:
* depends on signatures
* must detect then write new signatures then distribute them before they protect against a threat
* increase attack surface

However, it protects against the most common attacks out there on the internet

**Antivirus software** is just one piece of the our anti-malware defenses
* operates off a blacklist

**Binary whitelisting software** - implicit deny
* operates off a whitelist of known, trusted software
* typically only applies to executable binaries

Mechanisms:
* unique cryptographic hash of binaries used to identify unique binaries
* software-signing certificate

Code signing certificates increase attack surface

[system survival time](https://itknowledgeexchange.techtarget.com/security-corner/whats-your-systems-survival-time/)  
[Why disable antivirus](https://robert.ocallahan.org/2017/01/disable-your-antivirus-software-except.html)  
[Sophos Attack](https://lock.cmpxchg8b.com/Sophail.pdf)  
[binary whitelisting bypass](https://www.crn.com/news/security/240148192/bit9-admits-systems-breach-stolen-code-signing-certificates.htm)

## Disk Encryption
**Full-disk encryption (FDE)** - works by automatically converting data on a hard drive into a form that cannot be understood by anyone who doesn't have the key to "undo" the conversation

unencrypted partition for things like boot / kernel can be attacked

Secure Boot Protocol
* initially configured with a platform key (the public key corresponding to the private key to sign the boot files)

When you implement a full disk encryption solution at scale, it's super important to think about how to handle cases where passwords are forgotten.

**Key Escrow** - allows the encryption key to be securely stored for later retrieval by an authorized party

**Home directory** or **file-based encryption** only guarantees confidentiality and integrity of files protected by encryption

* [bitlocker](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview)
* [filevault2](https://support.apple.com/en-us/HT204837)
* [dm-crypt](https://wiki.archlinux.org/index.php/dm-crypt)
* [PGP](https://www.symantec.com/products/encryption)
* [TrueCrypt](http://truecrypt.sourceforge.net/)
* [VeraCrypt](https://www.veracrypt.fr/en/Home.html)


# Application Hardening
## Software Patch Management
