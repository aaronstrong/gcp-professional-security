# Encryption on GCP

**Table of contents:**
- [Encryption on GCP](#encryption)
    - [Encryption default at rest](#encryption-default-at-rest)
        - [Layers of Encryption](#layers-of-encryption)
        - [Encryption at the Hardware and Infrastructure layer](#hardware-and-infrastructure)
    - [Key Management](#key-management)
    - [Crypto Library](#crypto-library)
    - [CSEK](#csek)
- [Encryption in Transit](#encryption-in-transit)


<a id="encryption"></a>
# [Encryption Overview](https://cloud.google.com/docs/security/encryption/default-encryption)

Encryption at rest is encryption that is used to help protect data that is stored on a disk (including solid-state drives) or backup media. All data that is stored by Google is encrypted at the storage layer using the Advanced Encryption Standard (AES) algorithm, **AES-256**. We use a common cryptographic library, Tink, which includes our FIPS 140-2 validated module (named **BoringCrypto**) to implement encryption consistently across Google Cloud.


<a id="encryption-default-at-rest"></a>
## Default Encryption of data at rest
Google encrypts all customer content stored at rest, without any action from you, using one or more encryption mechanisms. 

<a id="layers-of-encryption"></a>
## Layers of Encryption

Google uses several layers of encryption to help protect data. Using multiple layers of encryption adds redundant data protection and allows us to select the optimal approach based on application requirements.

![](https://cloud.google.com/static/docs/security/encryption/default-encryption/resources/encryption-layers.svg?dcb_=0.6528154237885637)

<a id="hardware-and-infrastructure"></a>
### Encryption at the hardware and infrastructure layer

All of Google's storage systems use a similar encryption architecture, though implementation details differ from system to system. Data is broken into subfile chunks for storage.  Each chunk is encrypted at the storage level with an individual data encryption key (DEK): two chunks won't have the same DEK, even if they are owned by the same customer or stored on the same machine.

If a chunk of data is updated, it is encrypted with a new key, rather than by reusing the existing key. This partitioning of data, each using a different key, limits the risk of a potential data encryption key compromise to only that data chunk.

Google encrypts data before it is written to a database storage system or hardware disk. Encryption is inherent in all of our storage systems, rather than added afterward.

We use the AES algorithm to encrypt data at rest. All data at the storage level is encrypted by DEKs, which use AES-256 by default, with the exception of a small number of Persistent Disks that were created before 2015 that use AES-128.

<a id="storage-device-encryption"></a>
### Encryption at the storage device layer

In addition to storage system level encryption, data is also encrypted at the storage device level with AES-256 for hard disk drives (HDD) and solid-state drives (SSD), using a separate device-level key (which is different from the key used to encrypt the data at the storage level). A small number of legacy HDDs use AES-128. SSDs used by Google implement AES-256 for user data exclusively.

<a id="backup-encryption"></a>
### Encryption of backups

Our backup system ensures that data remains encrypted throughout the backup process. The backup system further encrypts most backup files independently with their own DEK. The DEK is derived from a key that is stored in Keystore and a randomly generated per-file seed at backup time. Another DEK is used for all metadata in backups, which is also stored in Keystore.

<a id="key-management"></a>
## Key Management

Because of the high volume of keys at Google, and the need for low latency and high availability, DEKs are stored near the data that they encrypt. DEKs are encrypted with (wrapped by) a key encryption key (KEK), using a technique known as envelope encryption. These KEKs are not specific to customers; instead, one or more KEKs exist for each service.

### What is Cloud KMS
- A central repository for storking KEKs
- Generate, use, rotate, and destroy symmetric encryption keys
- KEKs are not exportable from KMS
- Encryption and decryption done with keys in KMS
- Helps prevent misuse and provide an audit trail
- Automatically rotates KEKs at reguarl intervals, default is 90 days.
- Tracked every time it isused and authenticated and logged

<a id="crypto-library"></a>
## Google's common cryptographic library

Google's common cryptographic library is Tink, which incorporates our FIPS 140-2 validated module, BoringCrypto. Tink is available to all Google developers. The Tink encryption library supports a wide variety of encryption key types and modes, and these are reviewed regularly to ensure that they are current with the latest attacks.

<a id="csek"></a>
## Customer-Supplied Encryption Keys (CSEKs)

Keep encryption keys on premises where required, and use them to encrypt services in Google cloud.

Customer can use existing encryption keys that they manage with the GCP in combination with CSEKs.

>**Note**: Only available for Cloud Storage and Compute Engine

- Control access to and use of data at the finest level of granularity, ensuring data security and privacy.
- Manage encryption of cloud-hosted data the same way as currently do on-prem.


<a id="encryption-in-transit"></a>
# [Encryption in Transit](https://cloud.google.com/docs/security/encryption-in-transit)

Encryption in transit protects your data if communications are intercepted while data moves between your site and the cloud provider or between two services.

- Removing the need to trust the lower layers of the network which are commonly provided by third parties
- Reducing the potential attack surface
- Preventing attakcers from accessing data if communcations are intercepted

![](https://cloud.google.com/static/images/security/whitepaper-transit-01.svg)

<a id="phyical-boundary"></a>
## Physical Boundary

- Barrier to physical space that is controlled by or on behalf of Google
- Google ensures that rigourous security measures are in palce at the boundaries
- Automatically enforces additional protections outside of Google's physical trust boundary

<a id="traffic-routing"></a>
## Traffic Routing

Google Cloud services accept requests from around the world using a globally distributed system called the Google Front End (GFE). GFE terminates traffic for incoming HTTP(S), TCP and TLS proxy traffic, provides DDoS attack countermeasures, and routes and load balances traffic to the Google Cloud services themselves. There are GFE points of presence around the globe with routes advertised via unicast or Anycast.

GFEs route the user's request over our network backbone to a Google Cloud service. This connection is authenticated and encrypted from GFE to the front-end of the Google Cloud service or customer application, when those communications leave a physical boundary controlled by Google or on behalf of Google.

<a id="encryption-in-transit"></a>
## Encryption in Transit by Default

Figures 2 and 3 below illustrate the optional and default protections Google Cloud has in place for layers 3, 4, and 7.

Figure 2:
![](https://cloud.google.com/static/images/security/whitepaper-transit-02.svg)

Figure 3:
![](https://cloud.google.com/static/images/security/whitepaper-transit-03.svg)

<a id="methods-of-encryption"></a>
### Methods of Encryption

- Transport Layer Security (TLS)
    - Any data sent to GFE is encrypted in transitusing TLS (default)
    - Also used in API interactions
- BoringSSL
    - TOSL in the GFE is implemented with BoringSSL
    - Forked of OpenSSL
    - Automatically negotiaties the highest version of the protocol to ensure the best possible security
- Google's Certificate Authority
    - Identity verification achieved in TLS through the use of a certificate
    - Certificate holds DNS hostname and service and public key
- Root key migration and key rotation
    - Google is responsible for the rotation of keys and certificates
    - TLS certificates - rotated every 2 weeks
        * Lifetime of 3 months
    - Keys - rotated dailsy
        * Lifetime of 3 days
- Application Layer Transport Security (ALTS)
    - Layer 7 traffic
    - Mutual authentication and trnasport encypriton system developed by Google
    - Used for seucring Remote Procedure Call (RPC) communications within Google's infra.
    - Identities are bound to entities (user, machines, or service) instead of a specific server name or a host.
    - Relies on both the Handshake protocol and the Record Protocol
    - Governs how sessions are established, authenticated, encrypted, and resumed
    - GFE to service / service to service
    ![](https://cloud.google.com/static/images/security/alts-handshake.png)

