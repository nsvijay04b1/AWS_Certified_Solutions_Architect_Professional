# AWS CloudHSM:
- A hardware security module (HSM) is a computing device that processes cryptographic operations and provides secure storage for cryptographic keys. 
- AWS CloudHSM provides single-tenant HSMs in the AWS Cloud.
- Use cases:
	- Generate, store, import, export, and manage cryptographic keys, including symmetric keys and asymmetric key pairs.
	- Use symmetric and asymmetric algorithms to encrypt and decrypt data.
	- Cryptographically sign data (including code signing) and verify signatures.
	- Offload the SSL/TLS Processing for Web Servers.
	- Protect the Private Keys for a certificate issuing Certificate Authority (CA).
- CloudHSM Client SDK includes software to integrate your applications with your CloudHSM cluster.
- You can create a cluster that has from 1 to 28 HSMs in different AZs in the same Region. The default limit is 6 HSMs per account per region.
- Backup:
	- AWS CloudHSM makes periodic backups of the users, keys, and policies in the cluster. 
	- The HSM encrypts all of its data before sending it to a service-controlled S3 bucket. 
- FIPS 140-2 Level 3 certified.
- Pricing: number of hours.


## CloudHSM Architecture

- CloudHSM devices are deployed into a VPC managed by AWS
- They are injected into customer managed VPCs using ENIs (Elastic Network Interfaces)
- For HA we need to deploy multiple HSM devices and configure them as a cluster
- A client needs to be installed on the EC2 instances in order to be able to access HSM modules
- While AWS do provision the HSM devices, we as customers are responsible for the management of the customer keys
- AWS can provide software updates on the HSM devices, but these should not affect the encryption storage part

## CloudHSM Use Cases

- There is no native integration with AWS service, this means CloudHSM can not be used for S3 SSE
- CloudHSM can be used for client-side encryption before uploading data to S3
- CloudHSM can be used to offload SSL/TLS processing for web servers
- Oracle Databases from RDS can perform Transparent Data Encryption (TDE) using CloudHSM
- CloudHSM can be used to protect private keys for an Issuing Certificate Authority (CA)
