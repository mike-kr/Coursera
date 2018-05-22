# Symmetric Encryption

## Cryptography

Cryptography - hiding messages from potential enemies

Encryption - the act of taking a message, called **plaintext**, and applying an operation to it, called a **cipher**, so that you receive a garbled, unreadable message as the output, called **ciphertext**

Decryption - reverse process of encryption

Cipher made up of two components:

* **encryption algorithm** - the underlying logic of process that's used to convert the plaintext into ciphertext
* **key** - introduces something unique into cipher, otherwise anyone running the same encryption algorithm could decrypt the encryption

Security through obscurity

Kerchoff's principle:

* Cryptosystem - A collection of algorithms for key generation and encryption and decryption operations that compromise  a cryptographic service should remain secure - eve if everything about the system is known, except the key

Also referred to as:    Shannon's maxim or "The enemy knows the system"

The system should remain secure even if your adversary knows exactly what kind of encryption systems you're employing, as long as your **keys remain secure**

Cryptology - the overarching discipline that covers the practice of coding and hiding messages from third parties

Cryptanalysis - the opposite of cryptology looking for hidden messages or trying to decipher coded messages

**Frequency Analysis** - The practice of studying the frequency with which letters appear in a ciphertext

English most common letters:

* e, t, a, o

Most common pairs

* th, er, on, an

**Steganography** - the practice of hiding information from observers, but not encoding it

[number factorization](https://en.wikipedia.org/wiki/Integer_factorization)

## Symmetric Cryptography

**Symmetric-key algorithm** - uses same key to encrypt and decrypt messages

**Substitution cipher** - an encryption mechanism that replaces parts of your plaintext with ciphertext

* Caesar Cipher
  * ROT 13

**Stream cipher** - takes a stream of input and encrypts the stream one character or one digit at a time, outputting one encrypted character or digit at a time

**Block cipher** - the cipher takes data in,  places it into a bucket or block of data that's a fixed size, then encodes that entire block as one unit; extra space will be padded

Avoid key reuse - **Initialization vector (IV)**: bit of random data that's integrated into the encryption key and the resulting combined key is then used to encrypt the data

IV must be included in plaintext for decryption

## Symmetric Encryption Algorithms

**DES** - Data Encryption Standard

* Symmetric block cipher that uses 64-bit key sizes and operates on blocks 64-bits in size.  8-bits are used only for parity checking, a simple form of error checking, thus the real size is 56-bits 

**FIPS** - Federal Information Processing Standard

Key length - essentially defines the maximum limit of strength

Longer key lengths protect against brute-force attacks

**NIST** - National Institute of Standards & Technology

**AES** - Advanced Encryption Standard

* Symmetric like DES but uses 128-bit block sizes (twice that of DES) and supports key lengths of 128-bit, 192-bit, or 256-bit

Because of the large key size, brute-force attacks on AES are only **theoretical** right now, because the computing power required (or time required using modern technology) exceeds anything feasible today.

An important thing to keep in mind when considering various encryption algorithms is **speed** and **ease of implementation**.

**RC4 (Rivest Cipher 4)** - a symmetric stream cipher that gained widespread adoption because of its simplicity and speed

* supports key sizes from 48-bits to 2,048-bits
* safe from brute-force but inherent weakness and vulnerabilities are present
* RC4 NOMORE attack

Preferred TLS configuration: **TLS 1.2 with AES GCM**

* specific mode of operation for the AES block cipher that essentially turns it into a stream cipher
* GCM or Galois/Counter Mode - takes randomized seed value, incrementing this and encrypting the value, creating sequentially numbered blocks of ciphertexts.
  * The ciphertexts are then incorporated into the plain text to be encrypted

Pros of Symmetric Encryption

* Easy to implement and maintain
* One shared secret that you have to maintain and keep secure
* Fast and efficient at encrypting and decrypting large batches of data

Cons

* Only one shared secret
  * Maintenance / security when giving out secret

[RC4 NOMORE](https://www.rc4nomore.com/)

# Public Key or Asymmetric Encryption
## Asymmetric Cryptography
Asymmetric Encryption - Different keys are used to encrypt and decrypt  
* commonly used as key exchange mechanism to establish a shared secret that will be used with symmetric cipher

**Public Key Signatures** - verifies authenticity of message by combining the message, digital signature, and public key
* Public key should verify with the signature, otherwise the message shouldn't be trusted

Asymmetric Encryption grants us:
* Confidentiality - encryption decryption method
* Authenticity - digital signature
* Non-repudiation - author of message isn't able to dispute the origin of the message

Asymmetric encryption is more computationally expensive and complex  

**Message Authentication Codes (MACs)** - a bit of information that allows authentication of a received message, ensuring that the message came from the alleged sender and not a third party
* also ensures data integrity

**HMAC** - Keyed-hash message authentication code

**CMACs** - Cipher-Based Message Authentication Codes

**CBC-MAC** - Cipher block chaining message authentication codes

## Asymmetric Encryption Algorithms
**RSA** - Key generation uses 2 unique, very large, random, prime numbers

**DSA (Digital Signature Algorithm)** - Used for signing and verifying data part of FIPS
* depends on random seed value

**DH (Diffie-Hellman)** - key exchange algorithm

**PKI (Public Key Infrastructure**

**ECC (Elliptic curve cryptography)** - A public-key encryption system that uses the algebraic structure of elliptic curves over finite fields to generate secure keys

Both Diffie-Hellmen and DSA have elliptic curve variants, referred to as ECDH and ECDSA, respectively

[Sony PS3](https://nakedsecurity.sophos.com/2012/10/25/sony-ps3-hacked-for-good-master-keys-revealed/)
[Game Piracy](https://www.theguardian.com/technology/gamesblog/2011/jan/07/playstation-3-hack-ps3)

# Hashing
## Hashing
**Hashing (or a hash function)** -  a type of function or operation that takes in an arbitrary data input and maps it to an output of fixed size, called a hash or digest

You feed in any amount of data into a hash function and the resulting output will always be the same size, but the output should be **unique to the input**, such that two different inputs should never yield the same output.

Hashing can also be used to identify duplicate data sets in databases or archives to speed up searching of tables or to remove duplicate data to save space.

Cryptographic hashing is distinctly different from encryption because cryptographic hash functions should be one directional.  
Used for :
* authentication
* message integrity
* fingerprinting
* data corruption detection
* digital signatures
* etcetera

The ideal cryptographic hash function should be **deterministic**, meaning that the same input value should always return the same hash value.

**Hash collisions** - two different inputs mapping to the same output

Cryptographic hash functions operate on blocks of data like a Block Cipher
## Hashing Algorithms
**MD5**
* 512 bit blocks
* 128 bit hash digests
* Hash collision and vulnerabilities

**SHA1** - part of the Secure Hash Algorithm suite of functions, designed by the NSA, published in 1995
* recommended replacement of MD5
* 512 bit blocks
* 160 bit hash digest
* used in popular protocols like
  * TLS/SSL
  * PGP SSH
  * IPsec
  * Git

In 2010, NIST recommended to stop using SHA1 and rely on SHA2 or SHA3 instead

**MIC** (Message Integrity Check) - a hash digest of the message, like a check-sum
* doesn't use security keys so attacker could alter message
* protects against accidental corruption or loss
* doesn't protect against tampering or malicious actions
[Theoretical SHA1 Attacks](https://eprint.iacr.org/2005/010)
[SHA1 Broken](https://www.schneier.com/blog/archives/2005/02/sha1_broken.html)
[Partial Collision](https://eprint.iacr.org/2007/474)
[Full Collision](https://shattered.io/)
## Hashing Algorithms (continued)
Authentication
* store hash of password

A successful brute force attack, against even the most secure system imaginable, is a function of attacker time and resources

To raise difficulty, run password through hashing function multiple times (sometimes thousands)

**Rainbow Tables** - pre-computated table of all possible password values and their hashes

**Password Salt** - additional randomized data that's added into the hashing function to generate a hash that's unique to the password and salt combination

# Cryptography Applications
## Public Key Infrastructure
**PKI** Public key infrastructure - defines the creation, storage, and distribution of digital certificates

**Digital Certificate** - file that proves an entity owns a certain public key.  
Contains:
* Information on Public Key
* Registered Owner
* Digital Signature

**CA** (certificate authority) - responsible for storing, issuing, and signing certificates

**RA** (registration authority) - responsible for verify8ing the identities of any entities requesting certificates to be signed and stored with the CA.

 A central repository is needed to securely store and index keys, and a certificate management system of some sort makes managing access to stored certificates and issuance of certificates easier.

 SSL/TLS server certificate - certificate presents to a web server as part of the initial secure setup of an SSL/TLS connection

Wildcard certificate - hostname is replaced with an * to validate all hosts in a domain

Self-signed certificate - signed by the same entity issuing the certificate

SSL/TLS client certificate (optional) - as the name implies, these are certificates that are **bound to clients** and are used to **authenticate** the client to the server, allowing access control to an SSL/TLS service.
* usually the service operator would have their own internal CA which issues and manages client certificates for their own service

Code-signing certificates - this allows users of these signed applications to verify the signatures and ensure that the application was not tampered with
* used for signing executable programs
* verifies that the application came from the author and is not a malicious twin

PKI is built on trust.  
The chain of trust starts at the *Root Certificate Authority*  
* root certificates are self-signed because there is not a higher authority
* can sign a certificate and set the CA field to true, thus marking the certificate as an intermediary or subordinate CA.
  * this can be done down the chain

A certificate that has no authority as a CA is referred to as an **end-entity** or **leaf certificate**

Vendors ship trusted Root CA's with their OS to initiate the trust chain

The **X.509** standard is what defines the format of digital certificates
* also defines a **CRL** (certificate revocation list) which is a means to distribute a list of no longer valid certificates
* current modern version is version 3

Fields defined in X.509:
* Version - what version of the X.509 standard the certificate adheres to
* Serial number - a unique identifier for the certificate assigned by the CA which allows the CA to manage and identify individual certificates
* Certificate Signature Algorithm - this field indicates what public key algorithm is used for the public key and what hashing algorithm is used to sign the certificate
* Issuer Name - this field contains information about the authority that signed the certificate
* Validity - this contains two sub-fields - "Not Before" and "Not After" - which define the dates when the certificate is valid for
* Subject - this field contains identifying information about the entity the certificate was issued to
* Subject Public Key Info - These two sub-fields define the algorithm of the public key, along with the public key itself
* Certificate Signature Algorithm - same as the Subject Public Key Info field; these two fields must match
* Certificate Signature Value - the digital signature data itself

Certificate fingerprints - not included in the certificate itself but created by clients when validating or inspecting certificates
* just hash digests of whole certificate

Alternative to PKI is the **Web of Trust**
* individuals instead of CA's sign other's public keys
* Key signing parties

[X.509 Standard](https://www.ietf.org/rfc/rfc5280.txt)
## Cryptography in Action
HTTPS - the secure version of HTTP, the HyperText Transport Protocol

Encapsulates http over an SSL/TLS connection

SSL 3.0 deprecated in 2015 and TLS 1.2 is the current recommended revision

TLS is independent of HTTP and is a generic protocol to permit secure communications and authentication over a network

TLS is also used for securing other communications such as:
* VoIP calls
* email
* instant messaging
* Wi-Fi network security

TLS grants us 3 things:
1. A **secure** communication line, which means data being transmitted is protected from potential eavesdroppers
2. The ability to **authenticate** both parties communicating, though typically only the server is authenticated by the client
3. The **integrity** of communications, meaning there are checks to ensure that messages aren't lost or altered in transit

TLS Handshake
1. a client establishes a connection with a TLS enabled service, referred to in the protocol as **ClientHello**
  * Includes information about the client, like the version of the TLS that the client supports, a list of cipher suites that is supports, and maybe some additional TLS options
2. Server responds with a **ServerHello** message
  * selects the highest protocol version in common with the client, and chooses a cipher suite from the list to use.
  * It also transmits its digital signature and a final **ServerHelloDone** message.
3. The client then validates the certificate that the server sent over to ensure that it's trusted and it's for the appropriate host name.
4. Assuming certificate checks out, the client sends a **ClientKeyExchange** message.
5. The client then chooses a key exchange mechanism to securely establish a shared secret with the server which will be used with a symmetric encryption cipher to encrypt all further communications.  The client also sends a **ChangeCipherSpec** message indicating that it's switching to secure communications now that it has all the information needed to begin communicating over the secure channel.
6.  Finally an **encrypted Finished** message is sent which also serves to verify that the handshake completed successfully.

The session key is the shared symmetric encryption key used in TLS sessions to encrypt data being sent back and forth.

To defend against private key compromises, **Forward Secrecy** - a property of a cryptographic system so that even if a private key is compromised, the session keys are still safe

**SSH** (Secure Shell) - A secure network protocol that uses encryption to allow access to a network service over unsecured networks

**PGP** (Pretty Good Privacy) - An encryption application that allows authentication of data, along with privacy from third parties, relying upon asymmetric encryption to achieve this

[PGP](https://www.philzimmermann.com/EN/essays/WhyIWrotePGP.html)

## Securing Network Traffic
VPN (Virtual Private Network) - A mechanism that allows you to remotely connect a host or network to an internal, private network, passing the data over a public channel, like the internet

**IPsec** (Internet Protocol Security) - VPN protocol designed in conjunction with IPv6
* encrypts an IP packet, then encapsulates the encrypted packet with an IPsec packet
* IPsec packet is sent to VPN endpoint where it is decrypted and sent to its final destination
* supports 2 modes of operations
  * transport mode - only the payload of the IP packet is encrypted, leaving the IP headers untouched
  * tunnel mode - the entire IP packet, header payload and all, is encrypted and encapsulated inside a new IP packet with new headers

**L2TP** (Layer 2 Tunneling Protocol) - tunneling protocol that allows encapsulation of different protocols or traffic over a network that may not support the type of traffic being sent
* not a vpn solution
* doesn't provide encryption
* commonly used with IPsec
* can adjust, segregate, and manage the traffic

L2TPIPsec - combination of IPsec and L2TP

**Encapsulating Security Payload** - Part of the IPsec suite of protocols

The **tunnel** is provided by L2TP which permits the passing of unmodified packets from one network to another.  The **secure channel**, on the other hand, is provided by IPsec, which provides confidentiality, integrity, and authentication of data being passed.

**OpenVPN** - uses Open SSL Library
* can operate over either TCP or UDP, typically over port 1194
* relies on ethernet tap or layer 3 IP tunnel
* up to 256 bit 
* runs in userspace

[IETF RFC 3193](https://tools.ietf.org/html/rfc3193)  
[OpenVPN](https://openvpn.net/index.php/open-source.html)
## Cryptographic Hardware
**TPM** (Trusted Platform Module) - unique secret RSA key burned in during manufacturing
* Secure generation of keys
* Random number generation
* Remote attestation - system authenticating its software and hardware configuration to a remote system
* Data binding and sealing - using the secret key to derive a unique key that's then used for encryption of data

Mobile devices have something similar to TPM called **secure element**  

**TEE** (Trusted Execution Environment) provides a full-blown isolated execution environment that runs alongside the main OS

TPM most common use cases:
* ensure platform integrity, preventing unauthorized changes to the system either in software or hardware
* full disk encryption utilizing the TPM to protect the entire contents of the disk

**FDE** (Full Disk Encryption) - encrypts the entire disk, not just sensitive files and folders

Common Implementations:
* PGP
* Bitlocker (Microsoft)
* Filevault 2 (Apple)
* dm-crypt (linux)

Random numbers vs Pseudo-random

Entropy pool - source of random data to help seed random number generators

[Physical Attack on a TPM](https://gcn.com/Articles/2010/02/02/Black-Hat-chip-crack-020210.aspx)





