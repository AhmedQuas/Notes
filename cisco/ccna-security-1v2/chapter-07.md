### Spis treści
- [Chapter 7: Cryptographic Systems](#chapter-7-cryptographic-systems)
  - [7.1 Cryptographic Services](#71-cryptographic-services)
  - [7.2 Basic integirty and authenticity](#72-basic-integirty-and-authenticity)
  - [7.3 Confidentiality](#73-confidentiality)
  - [7.4 Public Key Cryptography](#74-public-key-cryptography)

# Chapter 7: Cryptographic Systems

## 7.1 Cryptographic Services

Three primary objectives of securing communications:
- `authentication` - guarantees that a message comes from the source that it claims to come from

    >Data nonrepudiation(niezaprzeczalność) is a similar service that allows the sender of a message to be uniquely identified. Sender cannot deny having been the source of that message.
- `integrity` - ensures that message is not altered in transit
- `confidentiality` - ensures privacy so that only the receiver can read the message

>The purpose of encryption and hashing is to guarantee confidentiality so that only authorized entities can read the message.

plaintext, cleartext - readable data

ciphertext - encrypted version of plaintext

key - link between the plaintext and ciphertext, it is required to encrypt and decrypt a message

cipher - algorithm used to encyrpt or decrypt message

Historical ciphers:
- scytale,
- caesar cipher - monoalphabetic substitution cipher
- vigenere cipher - polyalphabetic cipher
- enigma machine

Methods used in cipher algorithm:
- `transposition` - letters are not replaced, they are simply rearranged(rail fence)
- `substitution` - replace letter by another, retain the letter frequency of the original message
- `one-time pad` - RC4

Cryptoanalysis - cracking the code

Cryptoanalysis methods:
- brute-force method
- ciphertext method - attacker has the cipher of several encrypted messages
- known-plaintext method
- chosen-plaintext method
- chosen-ciphertext method
- meet-in-the-middle method
- frequency analysis

>The objective of modern cryptographers is to have a keyspace large enough that it takes too much time and money to accomplish a brute-force attack.

Cryptology = cryptography(development) + cryptoanalysis(breaking)

>It is an ironic fact of cryptography that it is impossible to prove that any algorithm is secure. It can only be proven that it is not vulnerable to known cryptanalytic attacks.

>`Old` encryption algorithms, such as the Caesar cipher or the Enigma machine, were `based on the secrecy of the algorithm` to achieve confidentiality. With `modern` technology, where reverse engineering is often simple, public-domain algorithms are frequently used. With most modern algorithms, successful decryption requires knowledge of the appropriate cryptographic keys. This means that the `security of encryption lies in the secrecy of the keys`, not the algorithm.

| Integrity | Authentication | Confidentiality |
|:---:      |:---:           |:---:            |
|   MD5     |    HMAC-MD5    |       DES       |
|   SHA     |    HMAC-SHA-1  |       3DES      |
|           |    RSA & DSA   |       AES       |

## 7.2 Basic integirty and authenticity

`Hash function` transforms a string of characters into a usually shorter, `fixed-length` value or key that represents the original string.(Hashed data is simply there for comparison)

Hashes are used for integrity assurance. As shown in the figure, a hash function takes binary data, called the message, and produces a fixed-length, condensed representation, called the hash. The resulting hash is also sometimes called the message digest, digest, or digital fingerprint. Hashing is based on a one-way mathematical function that is relatively easy to compute, but significantly harder to reverse.

>Hashing is similar to calculating cyclic redundancy check (`CRC`) checksums, but it is much stronger cryptographically.

Because of this, cryptographic hash values are often called `digital fingerprints`. They can be used to detect duplicate data files, file version changes, and similar applications. These values are used to guard against an accidental or intentional change to the data and accidental data corruption.

Hash function use case:
- to provide proof of authenticity when it is used with a symetric authentication key: IPSec or routing protocol authentication
- to provide authentication by generating one-time and one-way responses to challenges in authentication protocols such as the PPP Challenge Handshake Authentication Protocol (`CHAP`)
- to provide message integrity check proof, such as those used in digitally signed contracts, and public key infrastructure (PKI) certificates

Hash features, H(x) is:
- output has a fixed length
- relatively easy to compute for any given x
- one way and not reversible
- `collision free` - it is hard to find two different input values that result in the same hash value

>It is not possible for H(x) to be collision free if the size of the input is bigger than the size of the output.

>Hash functions are helpful when ensuring data is `not changed` accidentally, such as by a communication error.

There is no unique identifying information from the sender in the hashing procedure. This means that anyone can compute a hash for any data, as long as they have the correct hash function.

Well-known hash functions:
- MD5 - is essentially a complex sequence of simple binary operations, such as exclusive OR (XOR) and rotations, which are performed on input data and produce a 128-bit hashed message diges. MD5 is now considered a legacy algorithm and should be avoided
- SHA
  - SHA-1 - 160 bit
  - SHA-2:
    - SHA-224
    - SHA-256
    - SHA-384
    - SHA-512

>Remember that the longer the hash values are, the more secure they are.

Calculate md5sum of IOS

```
R1# verify /md5 flash:c1900-universalk9-mz.SPA.154-3.M2.bin
```

`HMAC` - Key-hash message authentication code(`KHMAC`)

>HMACs use an additional secret key(known by sender and receiver) as input to the hash function. This adds authentication to integrity assurance.

>The cryptographic strength of the HMAC depends on the cryptographic strength of the underlying hash function, on the size and quality of the key, and the size of the hash output length in bits.

>Care must be taken to distribute secret keys only to those who require the key.

IPsec VPNs rely on HMAC functions to authenticate the origin of every packet and provide data integrity checking.

Hashing in Cisco products:
- authentication information to routing protocols updates,
- packet integrity and authenticity in IPsec
- checksum for ISO images

>Digital signatures are an alternative to HMAC.

Key management:
- `key generation` - good random number generators
- `key verification` - almost all cryptographic algorithms have some weak keys that should not be used(Caesar key 0 or 25 does not encrypt the message)
- `key exchange` - provide secure agreement on the keying material with the other party over an untrusted medium
- `key storage` - malware, ransomware
- `key lifetime` - using short ey lifetime improves the security of legacy ciphers that are used on high-speed connections
- `key revocation and destruction` - notify all interested parties that a certain key has been compromised and should no longer be used

>In practice, most attacks on cryptographic systems are aimed at the key management level, rather than at the cryptographic algorithm itself.

`key length` = key size

`keyspace` - number of possible values that can be generated by a specific key length

>As key length increase, the keyspace increases exponentially (x^2). By adding one bit to the key, the keyspace is effectively doubled.

Types of cryptographic keys:
- `symmetric keys` - can be exchanged between two routers supporting a VPN
- `asymmetric keys` - used in secure HTTPS applications
- `digital signatures` - used when connecting to a secure website
- `hash keys` - used in symmetric and asymmetric key generation, digital signatures, and other types of applications

>With modern algorithms that are trusted, the strength of protection depends solely on the size of the key.


## 7.3 Confidentiality

Approaches to ensuring the security of data when using encryption:
- `protect the algorithm` - if the algorithm is revealed, every party that is involved must change the algorithm
- `protect the keys`

SSL/TLS => Session layer in OSI model

| Symetric encryption algortithm | Asymetric encryption algortithm |
|:---:      |:---:           |
|shared-secret key algorithms|public key algorithms|
|usual key length is 80-256 bits|usual key length is 512-4096 bits|
|sender and receiver must share a secret key|sender and receiver do not share secret key|
|quite fast(wire speed), they are based on simple mathematical operations|relatively slow because they are based on difficult computional algorithms|
|DES, 3DES, AES, IDEA, SEAL,RC2/4/5/6 & Blowfish|RSA, ElGamal, elliptic curves & DH|

`Block ciphers` - transform a fixed-length block of plaintext into a common block of ciphertext. Block size refers to how much data is encrypted at any one time.

`Stream ciphers` - encrypt plaintext one byte or one bit at a time

>Stream ciphers can be much faster than block ciphers, and generally do not increase the message size, because they can encrypt an arbitrary number of bits.

A5 cipher is used to encrypt GSM.

Criteria that should be considered when selecting an encryption algorithm:
- the algorithm is trusted by the cryptographic community
- the algorithm adequately protects against brute-force attacks
- the algorithm supports variable and long key lengths and scalability
- the algorithm does not have export or import restrictions

`DES` - Data Encryption Standard is a legacy(not recomended) symmetric encryption algorithm that usually operates in block mode by encrypting data in 64-bit blocks, 64 bit key(56+8 odd parity)

If you need to use DES you should consider about:
- changing keys frequently to help prevent brute-force attacks
- use secret channel to communicate the DES key
- use DES in CBC(Cipher Block Chaining) mode

    >With CBC, the encryption of each 64-bit block depends on previous blocks. CBC is the most widely used mode of DES.

- test a key to see if it is a weak key. DES has 4 weak & 12 semi-weak keys

`3DES` - DES three times in a row with different keys. Very secure but it is also resource intensive. 112(K1=K3), 168 bit keys

`3DES-EDE` - 3DES Encrypt Decrypt Encrypt

`AES` - Advanced Encryption Standard, 128, 192, 256 bit keys

>It can be used in high-throughput, low-latency environments, especially when 3DES cannot handle the throughput or latency requirements.

`SEAL` - Software-Optimized Encryption Algorithm, stream cipher with 160 bit key

>SEAL has several restrictions:
>- The Cisco router and the peer must support IPsec.
>- The Cisco router and the other peer must run an IOS image that supports encryption. These IOS images are identified with the string “k9” in the IOS filename.
>- The router and the peer must not have hardware IPsec encryption.

>The `RC` algorithms were designed all or in part by `Ronald Rivest`, who also invented `MD5`. The RC algorithms are widely deployed in many networking applications because of their favorable speed and variable key-length capabilities.

>In general RC algorithms are considered weak and should be avoided.

`DH` - Diffie-Hellman is not an encryption mechanism and is not typically used to encrypt data. Instead, it is a method to securely exchange the keys that encrypt data.

DH is commonly used when data is exchanged using an IPsec VPN, when data is encrypted on the Internet using either SSL or TLS, or when SSH data is exchanged.

>Unfortunately, `asymmetric key systems are extremely slow` for any sort of bulk encryption. This is why it is common to encrypt the bulk of the traffic using a symmetric algorithm, such as 3DES or AES and use the DH algorithm to create keys that will be used by the encryption algorithm.

## 7.4 Public Key Cryptography

The Public Key Infrastructure (`PKI`) is the framework used to securely exchange information between parties. The foundation of a PKI identifies a certificate authority which issues digital certificates that authenticate the identity of organizations and users. These certificates are also used to sign messages to ensure that the messages have not been tampered with.

Examples of CAs:
- Symantec Group(VeriSign),
- Comodo,
- Go Daddy,
- GlobalSign,
- DigiCert,

PKI Elements:
- `certificate store` - resides on a local computer and stores issued certificated and private keys
- `PKI certificate` - contain an entity's or individual's public key
- `PKI Certificate Authority` - CA is a trusted third party that issues PKI certificated to entities and individuals after veryfing their identity. It signs these certificated using its private key
- `Certificate Database` - stores all certificates approved by the CA

Classes of certificates:
- `0` - testing purpouses in which no checks have been performed
- `1` - inviduals with focus on verification email
- `2` - organizations for which proof of identity os required
- `3` - servers and software signing for which independent verification and checking of identity and authority is done by issuing certificate authority
- `4` - online business transactions between companies
- `5` - private organizations or governmental security

An enterprise can also implement PKI for internal use. PKI can be used to authenticate employees who are accessing the network. In this case, the enterprise is its own CA.

>IETF PKI X.509(`PKIX`) workgroup has published the Internet X.509 Public Key Infrastructure Certificate Policy and Certification Practices Framework (RFC2527).

>`X.509` is a well-known standard that defines basic PKI formats, such as the certificate and certificate revocation list (CRL) format, to enable basic interoperability. Specifically, the X.509 version 3 (X.509v3) standard defines the format of a digital certificate.

`PKCS` - Public-Key Cryptography Standards, published by RSA Laboratories

`PKCS #5` - Password-Based Cryptography Standard
`PKCS #12` - Personal Information Exchange Syntax Standard

`SCEP` - Simple Certificate Enrollment Protocol, designed by IETF to make issuing and revocation of digital certificates as scalable as possible

PKI topologies:
- `single-root PKI` - single CA, simple but difficult to scale to a large environment, SPOF
- `cross-certified CA` - peer-to-peer model, individual CAs establishe trust relationship with other CAs by cross-certifying
- `hierarchical CA` - the highest level CA is called the root, it can issue certificates to end users and to a subordinate CA. The sub-CAs could be created to support various business units, domains or communities that they can trust each other

    > The benefits of this topology include increased `scalability and manageability`. This topology works well in most large organizations. However, it can be difficult to determine the chain of the signing process.
- `hybrid infrastructure`

`RA` - Registration Authority, can accept requests for enrollment in the PK. This will help reduce the burden on CAs in an environment that supports a large number of certificate transactions or where the CA is offline.

RA handle tasks such as:
- authentication of users when they enroll with the PKI,
- key generation for users that cannot generate their own keys
- distribution of certificates after enrollment

>It is important to note that the RA only has the power to accept registration requests and forward them to the CA. It is not allowed to issue certificates or publish CRLs. The CA is responsible for these functions.

>In the CA authentication procedure, the first step when contacting the PKI is to securely obtain a copy of the public key of the CA. The public key verifies all the certificates issued by the CA and is vital for the proper operation of the PKI.

>The public key, called the self-signed certificate, is also distributed in the form of a certificate issued by the CA itself. Only a root CA issues self-signed certificates.

---

<div>
<a href="chapter-06.md">Prev: Chapter 6</a>
</div>
<div align="right">
<a href="chapter-08.md">Next: Chapter 8</a>
</div>