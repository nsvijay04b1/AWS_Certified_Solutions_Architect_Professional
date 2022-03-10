# AWS Systems Manager (formerly known as SSM):
- An AWS service that you can use to view and manage your instances’ OS on AWS. 
- Capabilities:
	- Operations Management.
	- Application Management: create, manage, and deploy application configurations. Includes the « Parameter Store ».
	- Change Management: enterprise change management framework for requesting, approving, implementing, and reporting on operational changes to your application configuration and infrastructure. 
	- Node Management: OS/software inventory, patch management, troubleshooting, ..
	- Shared Resources (SSM Documents).
- SSM agent:
	- On EC2: installed by default in Amazon AMIs. Needs to be enabled and started.
	- Can also be installed on on-prem servers. This is called a Hybrid environment. Needs an Amazon provided certificate and activate code.
	- The EC2 instance needs to have an instance profile (IAM role) so that the agent can push data to SSM.
	- A managed instance is any EC2 instance or on-premises machine in your hybrid environment that has been configured for Systems Manager. 
- SSM Parameter Store:
	- Provides secure, hierarchical storage for configuration data and secrets management. 
	- You can store data such as passwords, database strings, EC2 instance IDs and Amazon Machine Image (AMI) IDs, and license codes as parameter values.
	- You can store values as plain text or encrypted data using a KMS CMK (SecureString parameter type).
	- Parameters are retrieved using the GetParameter API call to ssm.{region}.amazonaws.com.
	- The parameters hierarchy is similar to a file path.
	- You can choose to encrypt your SecureString parameters using either:
		- your own customer-managed KMS CMK: you need to add an IAM permission to the user to use KMS operations on this KMS key.
		- or the default AWS-managed KMS CMK that SSM create for you in your account: all users within the account have access to this key, unless explicitly denied.
	- You can use AWS CloudTrail to monitor SecureString parameter activities. 
- SSM Inventory:
	- Collects metadata from your instances: OS, applications, files, network, …etc.
	- You can also collect custom inventory metadata.
	- To assign custom inventory to an instance, you can either store this metadata in a JSON file on your instance or use the Systems Manager PutInventory API action.
	- The shortest period for inventory is 30mn.
	- The inventory configuration is called an SSM association. Creating and association gives you an association ID and code.
	- The collected data can be stored in S3.
- SSM Run Command 
	- Enables you to automate common administrative tasks and perform ad hoc configuration changes at scale. 
	- Examples: install or bootstrap applications, build a deployment pipeline, capture log files when an instance is terminated from an Auto Scaling group, and join instances to a Windows domain.
- SSM document:
	- Defines the actions that Systems Manager performs on your managed instances. 
	- Systems Manager includes more than 100 pre-configured documents that you can use by specifying parameters at runtime.
	- Documents use JSON or YAML formats, and they include steps and parameters that you specify. 
	- Command/Automation documents: Used by "Run Command" to run commands. Used by "State Manager" to apply a configuration. Used by "Maintenance Windows" to apply a configuration based on the specified schedule. 
	- Package documents: Used by "Distributor" to install software on managed instances.
	- Session document: Session Manager uses session documents to determine which type of session to start.
	- CloudFormation document: you can choose to store your AWS CloudFormation templates in SSM.
- SSM Patch Manager:
	- You can use Patch Manager to apply patches for operating systems and for applications.
	- Patch Manager uses patch baselines, which include rules for auto-approving patches within days of their release, as well as a list of approved and rejected patches.
	- Can target individual instances or a group of instances. Can target instances having specific tags.
	- Can scan your instances and report compliance on a schedule, install available patches on a schedule, and patch or scan instances on demand whenever you need to. 
	- You create Maintenance Windows to run patching.
	- Uses "Run Command" in the background.
- SSM Change Calendar: lets you set up date and time ranges when actions you specify (for example, in Systems Manager Automation documents) may or may not be performed in your AWS account. 
- SSM Session Manager: lets you connect to your instances without using a bastion host.


# AWS AppConfig:
- A capability of AWS Systems Manager, to create, manage, and quickly deploy application configurations.
- You can use AWS AppConfig with applications hosted on Amazon Elastic Compute Cloud (Amazon EC2) instances, AWS Lambda, containers, mobile applications, or IoT devices. 

## Agent Architecture

- An instances needs the SSM agent to be installed in order to be able to be managed by the service
- It also needs an EC2 instance role attached to it which allows communication with the service
- Instances require connectivity to the SSM service. This can be done via IGW or VPCE
- On the on-premises side we need to create managed instance activations
- For each activation we will receive an activation code and an activation ID

## SSM Run Command

- It allows us to run commands on managed instances
- It takes a Command Document and executes it using the agent installed on the instance
- It does this without using SSH/RDP protocol
- Command documents can be executed on individual instances, multiple instances based on tags or resource groups
- Command documents can be reused and they can have input parameters
- Rate Control: if we are running commands on lot of instances, we can control it by using rate control. It can be based on:
    - Concurrency: on how many instances must run the command at a time
    - Error Threshold: defines how many individual commands can fail
- Output of commands can be sent to S3 or we can send SNS notifications
- Commands can be integrated with EventBridge

## SSM Patch Manager

- Allows to patch Linux and Windows instances running in EC2 or on-premises
- Concepts:
    - **Patch Baseline**: we can have many of these defined. Defines what should be installed (what patches, what hot-fixes)
    - **Patch Groups**: what resources we want to patch
    - **Maintenance Windows**: time slots when patches can take place
    - **Run Command**: base level functionality to perform the patching process. The command used for patching is `AWS-RunPatchbaseline`
    - **Concurrency** and **Error Threshold**: (see above)
    - **Compliance**: after patches are applied, system manager can check success of compliance compared to what is expected
- Patch Baselines:
    - For Linux: `AWS-[OS]DefaultPatchBaseline` - explicitly defines patches, example: `AWS-AmazonLinux2DefaultPatchBaseline`, `AWS-UbuntuDefaultPatchBaseline` - contain security updates and any critical update
    - For Windows: `AWS-DefaultPatchBaseline`, `AWS-WindowsPredefinedPatchBaseline-OS`, `AWS-WindowsPredefinedPatchBaseline-OS-Application`

