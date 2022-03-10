# Amazon Cognito:
- Identity management solution for B2C or B2B apps for their customers—so a customer-targeted IAM and user directory solution. 
- This fully managed service scales to support hundreds of millions of users. 


Cognito User Pools:
- With Cognito user pool, your users can sign up and sign in to your web or mobile app.
- Your app users can sign in:
	- Either directly through a user pool,
	- or Federate through a third-party identity provider (IdP).
- User Pool:
	- A user pool is a user directory in Amazon Cognito User Pools. 
	- provides User directory management and user profiles.
- Supported federated Identity Providers (IdPs):
	- Social IdPs: Google, Facebook, Amazon and Apple.
	- OIDC IdPs.
	- SAML IdPs.
- Includes a built-in, customizable web UI to sign in users.
- Security features such as multi-factor authentication (MFA), checks for compromised credentials, account takeover protection, and phone and email verification.
- After successfully authenticating a user, Amazon Cognito User Pools issues JSON web tokens (JWT).
- Cognito User Pools tokens can be used to:
	- Secure and authorize access to your own APIs,
	- Exchange for AWS credentials. 
	- Directly in API calls to AWS API Gateway.
	- Directly in requests sent to AWS AppSync.
- Cognito User pools use cases:
	- Design sign-up and sign-in webpages for your app.
	- Access and manage user data.
	- Track user device, location, and IP address, and adapt to sign-in requests of different risk levels.
	- Use a custom authentication flow for your app.


Amazon Cognito identity pools (federated identities):
- Enables identity federation to allow access to AWS services (authorization) for federated users.
- This is a more evolved service than the IAM identity federation as Cognito can interact with AWS STS and provide directly the AWS access token to the user app. 
- Supported IdPs:
	- Public providers: Amazon, Facebook, Google, Apple.
	- Amazon Cognito User Pools.
	- OIDC IdPs
	- SAML IdPs
- Auth Flow:
	- Your app authenticates to the IdP and gets a token from this IdP.
	- Your app calls GetId to Cognito Identity Pools which returns an identity.
	- Your app calls GetCredentialsForIdentity to Cognito Identity Pools which calls AWS STS on behalf of the user and returns the STS token to your app.
- You can use IAM policies to control access to AWS resources through Amazon Cognito identity pools based on user attributes (ABAC). 
- You can map attributes, within providers’ access and ID tokens or SAML assertions, to tags that can be referenced in the IAM permissions policies. 
- Cognito identity pools use cases:
	- Give your users access to AWS resources, such as an S3 bucket or a DynamoDB table.
	- Generate temporary AWS credentials for unauthenticated users.
- You need to configure your SAML identity provider to have Amazon Cognito as a relying part