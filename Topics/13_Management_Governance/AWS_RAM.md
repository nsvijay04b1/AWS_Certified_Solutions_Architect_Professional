# AWS Resource Access Manager (AWS RAM):
- Lets you share your resources with any AWS account or through AWS Organizations.
- If you have multiple AWS accounts, you can create resources centrally and use AWS RAM to share those resources with other accounts. 
- When you share a resource with another account, then that account is granted access to the resource. Any policies and permissions that apply to the account with which you have shared the resource apply to the shared resource. 
- When the owner of a resource shares it with your account, you can access the shared resource just as you would if it was owned by your account. 
- To be able to share resources within an AWS Organization, the management account should enable resource sharing.
- To start sharing a resource, you create a "Resource Share", add the resources to share, and specify the principals with whom they are to be shared