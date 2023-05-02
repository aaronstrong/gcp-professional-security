# Encryption on GCP

**Table of contents:**
- [Encryption on GCP](#encryption)
- [Encryption default at rest](#encryption-default-at-rest)
    - [Layers of Encryption](#layers-of-encryption)
    - [Encryption at the Hardware and Infrastructure layer](#hardware-and-infrastructure)
- [Key Management](#key-management)
- [Crypto Library](#crypto-library)


<a id="encryption"></a>
## [Encryption Overview](https://cloud.google.com/docs/security/encryption/default-encryption)

Encryption at rest is encryption that is used to help protect data that is stored on a disk (including solid-state drives) or backup media. All data that is stored by Google is encrypted at the storage layer using the Advanced Encryption Standard (AES) algorithm, **AES-256**. We use a common cryptographic library, Tink, which includes our FIPS 140-2 validated module (named **BoringCrypto**) to implement encryption consistently across Google Cloud.


<a id="encryption-default-at-rest"></a>
## Default Encryption of data at rest
Google encrypts all customer content stored at rest, without any action from you, using one or more encryption mechanisms. 

<a id="layers-of-encryption"></a>
### Layers of Encryption

Google uses several layers of encryption to help protect data. Using multiple layers of encryption adds redundant data protection and allows us to select the optimal approach based on application requirements.

![](https://cloud.google.com/static/docs/security/encryption/default-encryption/resources/encryption-layers.svg?dcb_=0.6528154237885637)

<a id="hardware-and-infrastructure"></a>
#### Encryption at the hardware and infrastructure layer

All of Google's storage systems use a similar encryption architecture, though implementation details differ from system to system. Data is broken into subfile chunks for storage.  Each chunk is encrypted at the storage level with an individual data encryption key (DEK): two chunks won't have the same DEK, even if they are owned by the same customer or stored on the same machine.

If a chunk of data is updated, it is encrypted with a new key, rather than by reusing the existing key. This partitioning of data, each using a different key, limits the risk of a potential data encryption key compromise to only that data chunk.

Google encrypts data before it is written to a database storage system or hardware disk. Encryption is inherent in all of our storage systems, rather than added afterward.

We use the AES algorithm to encrypt data at rest. All data at the storage level is encrypted by DEKs, which use AES-256 by default, with the exception of a small number of Persistent Disks that were created before 2015 that use AES-128.

<a id="key-management"></a>
### Key Management

Because of the high volume of keys at Google, and the need for low latency and high availability, DEKs are stored near the data that they encrypt. DEKs are encrypted with (wrapped by) a key encryption key (KEK), using a technique known as envelope encryption. These KEKs are not specific to customers; instead, one or more KEKs exist for each service.

<a id="crypto-library"></a>
### Google's common cryptographic library

Google's common cryptographic library is Tink, which incorporates our FIPS 140-2 validated module, BoringCrypto. Tink is available to all Google developers. The Tink encryption library supports a wide variety of encryption key types and modes, and these are reviewed regularly to ensure that they are current with the latest attacks.