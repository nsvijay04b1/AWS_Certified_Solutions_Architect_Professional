# AWS Config:
- Enables you to assess, audit, and evaluate the configurations of your AWS resources. 
- You can review the changes history of your configurations.
- You can determine what the resource’s configuration looked like at any point in the past. 
- You can track relationships and dependencies between AWS resources prior to making changes.
- Configuration snapshot: a point-in-time capture of all your resources and their configurations. Generated on demand.
- AWS Config can monitor your configurations with the following workflow: Triggers ==> Rules ==> Notification ==> Remediation.
- Triggers: 
	- Conditions under which AWS Config runs your rules.
	- Two trigger types (not exclusive):
		- Configuration changes: AWS Config runs evaluations for the rule when certain types of resources are created, changed, or deleted. 
		- Periodic: AWS Config runs evaluations for the rule at a frequency that you choose.
- Rules: 
	- evaluation of recorded configurations against desired configurations.
	- AWS managed rules: predefined, customizable rules that AWS Config uses to evaluate whether your AWS resources comply with common best practices. 
	- Example of AWS Config Managed Rule: checks that all EBS volumes are encrypted.
	- Custom rules: you need to develop an AWS Lambda function, which contains the logic that evaluates whether your AWS resources comply with the rule. 
- Notification:
	- Configuration changes that deviate from your rules automatically trigger SNS notifications and Amazon CloudWatch events.
- Remediation:
	- You can use remediation actions.
	- A Conformance Pack = Rules + Remediation actions. 