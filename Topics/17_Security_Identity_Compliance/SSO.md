# AWS Single Sign-On (AWS SSO):
- A cloud-based single sign-on (SSO) service focused on SSO for employees when accessing AWS services or cloud apps.
- Identity Source:
	- You can have only ONE identity source per AWS organization. 
	- Supported identity sources:
		- AWS SSO integrated identity store.
		- Active Directory (either AWS Managed Microsoft AD or your self-managed Active Directory).
		- External SAML 2.0 IdP.
	- If your user directory is Active Directory, AWS SSO adds the SAML IdP capability to your AD.
	- Amazon Cognito is not a supported identity source in AWS SSO.
- Supported applications for SSO:
	- AWS Management Console. 
	- AWS SSO-integrated applications such as Amazon SageMaker or AWS IoT SiteWise.
	- Built-in SAML integrations to many popular SaaS applications such as Salesforce, Box, and Office 365.
	- Custom SAML 2.0 applications.
- User Portal:
	- AWS SSO supports users single sign-on to business applications through web browsers only. Not supported for mobile/desktop applications and API calls.
	- AWS SSO includes a user portal where your end-users can find and access all their assigned AWS accounts, cloud applications, and custom applications in one place. 
	- The user portal gives also users the ability to retrieve temporary credentials for the IAM role of a given AWS account so they can use it for short-term access to the AWS CLI. 
- Permission Sets:
	- A collection of policies that determine an SSO user's permissions to access a given AWS account.
	- Permission sets ultimately get created as IAM roles in a given AWS account, with trust policies that allow users to assume the role through AWS SSO. 
	- Can contain either AWS managed policies (job function policies) or custom policies that are stored in AWS SSO.
	- You can set the session duration: max 12 hours. Default 1 hour.
- Users follow a simple sign-in process to use the AWS Management Console:
	- Users use their directory credentials to sign in to the AWS SSO user portal.
	- Users then choose the AWS account name that will give them federated access to the AWS Management Console for that account.
	- Users who are assigned multiple permission sets choose which IAM role to use.
- Attribute-based access control (ABAC):
	- An authorization strategy that defines permissions based on attributes. 
	- You can use user attributes that come from any AWS SSO identity source to manage access to your AWS resources.
	- In AWS, these attributes are called tags. 
	- You can use access control attributes in your permission sets using the aws:PrincipalTag condition key for creating access control rules. 
- SSO access to Applications:
	- You can enable SSO access to applications using AWS SSO.
	- You create a trusted relationship between AWS SSO and the application's service provider. 
	- After the application has been successfully added to the AWS SSO console, you can manage which users or groups need permissions to the application.
	- Supports the following applications types:
		- AWS SSO-integrated applications (such as Amazon SageMaker or AWS IoT SiteWise).
		- Cloud applications (such as Salesforce, Box, Slack and Office 365).
		- Custom SAML 2.0 applications.
- AWS SSO Integrates deeply with the AWS Organizations service.
- Enabling AWS SSO, including enabling AWS Organizations, has no impact on the users, roles, or policies that you???re already managing in IAM.
- AWS SSO also supports the System for Cross-domain Identity Management (SCIM) standard for enabling automatic provisioning of users and groups from Azure AD or Okta Universal Directory to AWS. 

# SSO

- A way to use existing enterprise identity store with AWS
- Allows to centrally manage SSO access to multiple AWS accounts and external business applications as well
- Replaces the historical uses cases provided by SAML 2.0
- Flexible Identity store: where identities are stored. Allows for external identities to be swapped with AWS credentials
- AWS SSO supports:
    - Built-in identity store
    - AWS Managed Microsoft AD
    - On-premise Microsoft AD
    - External Identity Provider - SAML 2.0
- AWS SSO is preferred by AWS for any "workforce" (enterprise) identity federation over the traditional SAML 2.0 based identity federation

## AWS SSO Architecture

![AWS SSO](images/AWSSSO.png)