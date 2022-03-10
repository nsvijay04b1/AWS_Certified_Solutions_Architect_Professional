# AWS Certificate Manager (ACM):
- A service that lets you easily provision, manage, and deploy public and private SSL/TLS certificates for use with AWS services and your internal connected resources. 
- ACM makes it easy to import SSL/TLS certificates issued by third-party CAs and deploy them with your ELBs, CloudFront and API Gateways. 
- Certificates in ACM are regional resources. To use a certificate with ELB for the same FQDN in more than one AWS region, you must request or import a certificate for each region.
- To use an ACM certificate with Amazon CloudFront, you must request or import the certificate in the US East (N. Virginia) region. 
- Public and private certificates provisioned through AWS Certificate Manager for use with ACM-integrated services are free. 
- ACM provides managed renewal for your Amazon-issued SSL/TLS certificates. 
- ACM Private Certificate Authority (CA):
	- A managed private CA service that helps you easily and securely manage the lifecycle of your private certificates. 
	- You pay monthly for the operation of the private CA and for the private certificates you issue.


ACM vs IAM certificate store:
- For certificates in a Region supported by ACM, we recommend that you use ACM to provision, manage, and deploy your server certificates.
- In unsupported Regions, you must use IAM as a certificate manager. 
