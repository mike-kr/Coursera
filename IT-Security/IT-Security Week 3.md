# Authentication
## Authentication Best Practices
**Identification** - The idea of describing an entity uniquely

**Authentication (authn)** - proving Identification

**Authorization (authz)** - pertains to the resources an identity has access to

Use strong password - trade-off between security and usability  
Think of security as *Risk Mitigation*

Incorporating **good password policies** into an organization is key to ensuring that employees are securing their accounts with **strong passwords**

Good passwords policies check for:
* length requirements
* character complexity
* dictionary words
* do not write down or save passwords in plaintext
* do not share passwords
* do not reuse passwords
* password rotation policy*
  * too short a period, may encourage poor password practices
## Multifactor Authentication
**Multifactor Authentication** - a system where users are authenticated by presenting multiple pieces of information or objects
* Something you know = Password / Pin
* Something you have = ATM / Bank card
* Something you are = Biometric ID

Physical tokens:
* usb device with secret token on it
* stand alone device that generates a token
* key used with traditional lock

**OTP (One-time password)** - Physical tokens generate a short-lived token

**TOTP (Time-based token)** - seed value with current time generates a token
* requires device / server to be relatively time synchronized
  * NTP Network time protocol

**Counter-based Tokens** - uses secret seed value with a secret counter value incremented every time an OTP is generated

SMS - vulnerable

**Biometric authentication** - the process of using unique physiological characteristics of an individual to identify them
* biometric data stored as hashes
* facial recognition uses 2  cameras (one infrared for depth) so printouts cant fool the system

**U2F (Universal 2nd Factor)** - standard developed jointly by Google, Yubico, and NXP Semiconductors
* U2F tokens referred to as Security Keys
* Challenge-response mechanism with public key cryptography

[Creating Fake Fingerprints](http://www.planetbiometrics.com/article-details/i/5774/desc/indian-pupils-cheat-biometric-system-with-glue/)

## Certificates 
Client Certificates:  
In order to issue client certificates, an organization must setup and maintain CA infrastructure to issue and sign certificates.

Client authenticating the server = Mutual Authentication

Certificate revocation list (CRL) - a signed list published byt he CA which defines certificates that have been explicitly revoked

Prove possession of private key through challenge-response mechanism
* random bit of data to be signed by private key to verify with public key
## LDAP
**Lightweight Directory Access Protocol (LDAP)** - an open, industry-standard protocol for accessing and maintaining directory services

LDAP specification describes the data structure of the directory itself
* defines functions for interacting with the service, like performing lookups and modifying data

Uses a tree structure called a **Data information tree** 

Object-oriented

**Organizational Units (OUs)** - group related objects into units and allows for inheritance and nesting

**Distinguished Name (DN)** - unique identifier

Common Operations:
* **Bind** - how clients authenticate to the server

* **StartTLS** - permits client to communicate over LDAPv3 over TLS
* Search, for performing look ups and retrieval of records
* Add/delete/modify
* Unbind - closes connection to LDAP server

## RADIUS
**Remote Authentication Dial-In User Service (RADIUS)** - A protocol that provides AAA services for users on a network

**Extensible Authentication Protocol (EAP)**

Clients dont directly connect, they present credentials to a NAS (network access server) that then forwards the information to the RADIUS server

RADIUS server then verifies credentials using a configured authentication scheme.  It then replies with one of three messages:
* access reject
* access challenge
* access accept
## Kerberos
**Kerberos** - a network authentication protocol that uses "tickets" to allow entities to rove their identity over potentially insecure channels to provide mutual authentication
* uses symmetric encryption

Kerberos:
* User enters username / password on client machine
* their kerberos client software will then take the password and generate a symmetric encryption key from it
* client sends a plain text message to the Kerberos, AS or Authentication Server which includes the user ID of the authenticating user.  The password or secret key derived from the password aren't transmitted
* AS uses the user ID to check if there is an account in the authentication database. If so, the AS will generate the secret key using the hashed password stored in the ky distribution center server
* AS will then use the secret key to encrypt and send a message containing the client TGS session key.
  * This is a secret key used for encrypting communications with the Ticket Granting Service (TGS) which is already known by the AS
* AS also sends a second message with a Ticket Granting Ticket (TGT) which is encrypted using the TGS secret key
  * TGT has information like the client ID, ticket validity period, and the client taking granting service session key so the first message can be decrypted using the shared secret key derived from the user password.  It then provides the secret key that can decrypt the second message giving the client a valid Ticket Granting Ticket.
* requesting access to a service within the Kerberos realm is done by sending a message to the Ticket Granting Service with the encrypted Ticket Granting Ticket along with the service name or ID
  * it also sends a message containing an authenticator   that has the client ID and time stamp that's encrypted withe the client Ticket Granting Ticket session key from the AS
* The Ticket Granting Service decrypts the Ticket Granting Ticket using the Ticket Granting Service secret key which provides the Ticket Granting Service with the client Ticket Granting Service session key
* It used the session key to decrypt the authenticator message
* checks the client ID of the two messages to ensure they match
* if they match it sends two messages back to the client
  * first one contains the client to server ticket which is comprised of the client ID, client address, validity period, and the client-server session key encrypted using the service's secret key
  * second is a new authenticator with the client ID and time stamp encrypted using the client-server session key
* Client sends two messages to the SS (Service Server)
  * first, encrypted client to server ticket received from the Ticket Granting Service
  * second, new authenticator with the client ID and time stamp encrypted using the client-server session key
* The SS decrypts the first message using its secret key which provides it with the client-server session key
* The key is then used to decrypt the second message and it compares the client ID in the authenticator to the one included in the client to server ticket.
* If these match, then the SS sends a message containing the time stamp from the client supplied authenticator encrypted using the client-server session key.
* The client then decrypts this message and checks that the time stamp is correct  authenticating the server
* if this all succeeds, then the server grants access to the requested service on the client

Criticized because it's a single monolithic point of failure as well as time sensitive requiring client and server to be relatively synchronized (usually solved using NTP)  
It also has a troubling trust model - it requires the clients and services to have an established trust in the Kerberos server in order to authenticate using Kerberos meaning it's not possible for users to authenticate using Kerberos from unknown or untrusted clients
## TACACS+
**TACACS+** - Terminal Access Controller Access-Control System Plus
* primarily used for device administration, authentication, authorization, and accounting
* mainly used as an authentication system for network infrastructure
## Single Sign-On
**Single Sign-On (SSO)** - an authentication concept that allows users to authenticate once to be granted access to a lot of different services and applications
* accomplished by authenticating to a central authentication server like an LDAP server
* this then provides a cookie, or token that can be used to get access to applications configured to use SSO
* Kerberos is a good example 

Attackers may try to steal the SSO cookie or token
* this also allows attackers to bypass multi-factor authentication since the 

OpenID
# Authorization
## Authorization and Access Control Methods
**Authorization** - pertains to describing what the user account has access to, or doesn't have access to

OAuth
## Access Control
**OAuth** - an open standard that allows users to grant third-party websites and applications access to their information without sharing account credentials

Can be thought of as a form of access delegation

Authorization through Access tokens
* access tokens have a scope (i.e. only email)

OAuth permissions can be used in phishing-style attacks to gain access to accounts, **without requiring credentials** to be compromised

OpenID is an authentication system while OAuth is specifically authorization

OpenID Connect is an authentication layer built on top of OAuth 2.0 designed to improve upon OpenID and build better integration with OAuth authorizations

TACACS+ is a full AAA system - once a user is authenticated, users are allowed or disallowed access to run certain commands or access certain devices.  This allows admin access for users that administer devices while still allowing less privileged access to other users when necessary

RADIUS also allows authorization

[OAuth based worm attack](https://www.theverge.com/2017/5/3/15534768/google-docs-phishing-attack-share-this-document-with-you-spam)

## Access Control List
**Access Control List (ACL)** - a way of defining permissions or authorizations for objects

Most common - file permissions

Individual settings - Access Control entries
# Accounting
## Tracking Usage and Access
**Accounting** - keeping records of what resources and services your users accessed, or what they did when they were using your systems

**Auditing** - reviewing of records to ensure nothing is out of the ordinary

TACACS+ is a device access AAA system that manages who has access to your network devices and what they do on them.

RADIUS is a network access AAA system

