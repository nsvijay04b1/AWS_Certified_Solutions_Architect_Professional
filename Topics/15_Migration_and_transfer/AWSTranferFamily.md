# AWS Transfer Family:
- A secure transfer service that enables you to transfer files into and out of AWS S3 and EFS. 
- Supported protocols: SFTP, FTPS, FTP.
- You get started with AWS Transfer Family by creating a file transfer protocol-enabled server and then assigning users to use the server. 
- You can add users using the Transfer Family service-managed identity or a custom identity provider.
- Service-managed identity type:
	- You add users to your file transfer protocol enabled server.
	- As part of each user's properties, you also store that user's SSH public key. The private key is stored locally on your user's computer. 
	- You specify a user's home directory, or landing directory, and assign an AWS IAM role to the user.
	- Optionally, you can provide a scope-down policy to limit user access only to the home directory of your Amazon S3 bucket. 
- Custom Identity Provider type:
	- You provide a RESTful interface with a single Amazon API Gateway method. Transfer Family calls this method to authenticate your users.
	- One option is tu use a Lambda function behind the API gateway and use AWS Secrets Manager as your identity provider.
	- Supports WAF since Nov-2020: you can enable AWS WAF rules to filter requests based on end users’ source IP addresses or implement rate-based rules to slow down brute force attacks.

