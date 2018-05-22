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


