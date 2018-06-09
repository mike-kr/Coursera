# Fictional Organization

######About:

Small, but growing, employee base, with 50 employees in one small office.   The company is an online retailer of the world's finest artisanal, hand-crafted widgets.

###### Requirements:

* An external website permitting users to browse and purchase widgets
* An internal intranet website for employees to us
* Secure, remote access for engineering employees
* Reasonable, basic firewall rules
* Wireless coverage in the office
* Reasonably secure configurations for laptops
* Privacy, compliance, and other security policies

###### Checklist:

* Authentication system
* External website security
* Internal website security
* Remote access solution
* Firewall and basic rules recommendations
* Wireless security
* VLAN configuration recommendations
* Laptop security configuration
* Application policy recommendations
* Security and privacy policy recommendations
* Intrusion detection or prevention for systems containing customer data

## Authentication System

First, we'll define some terminology to understand the macro view of our recommendations:

**Identification**: The idea of describing an entity uniquely

**Authentication**: Proving *identification*

**Authorization**: Pertaining to the resources an identity has access to

###For identification, and authentication we recommend using **Microsoft Active Directory.**

  Active Directory's default authentication protocol, Kerberos, is a network authentication protocol utilizing the single sign-on authentication concept.  **Single Sign-On (SSO)** allows users to authenticate once to be granted access to many different services and applications.  The **Kerberos** protocol uses "tickets" to allow entities to prove their identity over potentially insecure channels to provide mutual authentication using symmetric encryption.  The specifics of how Kerberos operates can be found in its documentation. I have provided a link to the documentation below:

https://web.mit.edu/kerberos/krb5-latest/doc/

### Username / Password Policy

* Emphasize use of password manager
* Usernames/User IDs are case *insensitive* and unique.  
* Usernames/User IDs for high security assets could be assigned, rather than user created.
* If using an email address as a username, utilize input validation and ensure the address is deliverable.  The only way to do this is to send a confirmation email.  This provides a positive acknowledgment that the user has access to the mailbox and is likely but not guaranteed to be authorized to use it.  Email verification links should not initiate an authenticated session.
*  Password lengths should be no shorter than 10 characters and typically no longer than 128 characters.  Longer passwords can create a dos attack on the authentication server which can be mitigated somewhat by choosing scrypt or preferably argon2id over SHA functions for hashing.
* Passwords must include at least 1 of each of the following
  * uppercase character (A-Z)
  * lowercase character (a-z)
  * digit (0-9)
  * special character or punctuation (spaces are special characters too)
* Characters can not be used more than twice (2) in a row (e.g. 111 not allowed)
* Commonly used passwords are banned
* Multiple users must use different password topologies
* Require a minimum topology change between old and new passwords

