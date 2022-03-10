# AWS Key Management Service (KMS):
- A fully managed service to create and manage cryptographic keys.
- You can create symmetric or asymmetric keys.
- You can also import keys from your own key management infrastructure or use keys stored in your AWS CloudHSM cluster (custom key store). In both cases, only symmetric keys are supported. 
- Enforces permissions to use keys using AWS IAM.
- KMS keys management is a multi-tenant infrastructure (FIPS level 2). If you require a dedicated infrastructure (FIPS Level 3), use a custom key store (backed with a CloudHSM cluster).
- In KMS, a master key is called Customer Master Key (CMK).

## AWS Signature version 4:
- When you send HTTP requests to AWS services, you (or your client tool/SDK) sign the requests so that AWS can identify who sent them. 
- You sign requests with your AWS access key, which consists of an access key ID and secret access key.
- Some requests do not need to be signed, such as anonymous requests to Amazon S3.

## Envelope encryption:
- Envelope encryption is the practice of encrypting plaintext data with a data key, and then encrypting the data key under another key. 
- The latter key can also be encrypted under a third key, and so on.
- Eventually, one key must remain in plaintext so you can decrypt the keys and your data. 
- This top-level plaintext encryption key is known as the master key. 

Customer Master Keys (CMKs):
- CMK is a logical representation of a master key. The CMK includes metadata, such as the key ID, creation date, description, and key state. 
- You can use AWS KMS APIs directly to encrypt and decrypt data using your CMKs stored in the service (up to 4 KB of data per API call).
- Can be Symmetric or Asymmetric.
- Symmetric Keys cannot be exported.
- For Asymmetric keys, only the public key can be exported.
- Key identifiers (Key IDs) act as names for your CMKs.

Custom Key Store:
- KMS provides the option for you to create your own key store using HSMs that you own and manage (FIPS 140-2 Level 3 certified).
- Each custom key store is backed by an AWS CloudHSM cluster. 
- When you create a CMK in a custom key store, the service generates and stores key material for the CMK in the associated CloudHSM cluster. 

Asymmetric Keys:
- KMS provides you the capability to create and use asymmetric CMKs.
- You can designate a CMK for use as a signing key pair or an encryption key pair. 
- Asymmetric cryptographic operations using these CMKs are performed inside HSMs. You can request the public portion of the asymmetric CMK for use in your local applications, while the private portion never leaves the service.
- You can also generate an asymmetric data key pair. This operation returns a plaintext copy of the public key and private key.

Symmetric vs Asymmetric:
- AWS KMS supports symmetric and asymmetric CMKs. 
- A symmetric CMK represents a 256-bit key that is used for encryption and decryption. 
- An asymmetric CMK represents either:
	- an RSA key pair that is used for encryption and decryption or signing and verification (but not both),
	- an Elliptic Curve (ECC) key pair that is used for signing and verification. 

Customer managed CMKs: 
- You have full control over the management of your keys, including the ability to share access to keys across accounts or services.
- You can assign policies to these CMKs.
- You can define an alias and description for the key.
- You can opt-in to have the key automatically rotated once per year (for symmetric keys only).

AWS managed CMKs:
- Applies to some AWS services only.
- For these AWS services, data is encrypted by default using keys that are stored in your AWS KMS and managed by the AWS service in question. 
- Some services give you the choice of managing the keys yourself or allowing the service to manage the keys on your behalf. 
- Automatic rotation required every 3 years (for symmetric keys only).

AWS owned CMKs:
- a collection of CMKs that an AWS service owns and manages for use in multiple AWS accounts. 
- You cannot view or use them.

Key Material:
- Key Material: the 256-bit key that is included in a CMK and used to encrypt and decrypt data.
- By default, KMS generates the key material for the CMK you create.
- But you can create a CMK without key material and then import your own key material into that CMK, a feature often known as "bring your own key" (BYOK). 
- Importing Key Material:
	- Supported only for symmetric CMKs in AWS KMS key stores. It is not supported on asymmetric CMKs or CMKs in custom key stores. 
	- You cannot enable automatic key rotation for a CMK with imported key material. However, you can manually rotate a CMK with imported key material. 
	- Importing the same key material to two different CMKs does not make them identical CMKs.

Key Rotation:
- To create new cryptographic material for your CMK, you can do either manual or automatic rotation of the CMK.
- Automatic Rotation:
	- Supported only on symmetric customer-managed CMKs.
	- Rotation is done once per year without the need to re-encrypt previously encrypted data.
	- The properties of the CMK, including its key ID, key ARN, region, policies, and permissions, do not change when the key is rotated.
	- The service automatically keeps older versions of the master key available to decrypt previously encrypted data. 
- Manual Rotation:
	- You create a new CMK and then change your applications or aliases to use the new CMK.
	- The new CMK has as a different key ID and ARN.
	- You can use a KMS alias to refer to a CMK in your applications. You then need to only update the KMS alias when rotating the CMK.
	- Use cases: 
		- Control the key rotation schedule
		- Rotate CMKs that are not eligible for automatic key rotation: asymmetric CMKs, CMKs in custom key stores, and CMKs with imported key material.
	- You should keep the original CMK enabled: when decrypting data, KMS identifies the CMK that was used to encrypt the data, and it uses the same CMK to decrypt the data. 


Data keys: 
- Used to encrypt data locally in the AWS service or in your application. 
- Generated by KMS but must be used outside of KMS.
- Data keys are not retained or managed by AWS KMS. 
- Can be exported in plain text or encrypted under a symmetric CMK you define. 
- Can be symmetric (data key) or asymmetric (data key pair).
- Data keys used by AWS Services:
	- Some AWS services (like S3) encrypt your data and store an encrypted copy of the data key along with the encrypted data.
	- When the service needs to decrypt your data, it requests AWS KMS to decrypt the data key using your CMK.
	- If the user requesting data from the AWS service is authorized to decrypt under your CMK, the AWS service will receive the decrypted data key from AWS KMS. The AWS service then decrypts your data and returns it in plaintext. 

CMK Authorization:
- Key administrator: manages the key but cannot use it.
- Key User: can use the key.

KMS Service Access: 
- Access to KMS uses a public endpoint (Internet).
- If you want avoid traversing Internet to communicate with KMS, use a VPC Endpoint and enable the Private DNS Name option

### Key Concepts

- CMKs are isolated to a region and never leave KMS
- There are 2 types of CMKs: AWS managed (created automatically) and customer managed (much more configurable, can be accessed by other AWS accounts)
- CMKs support rotation. Rotation is optional for customer managed keys
- CMK contains the current backing key and previous backing keys caused by rotation
- We can create aliases for CMKs
- Key policies and security:
    - Key Policies (Resource): similar to an S3 bucket policy
    - Every CMK has a key policy