# The 7 Rs of application migration:
	- Rehost (lift and shift): Move an application to the cloud without making any changes: from a VM/Server to an EC2 instance.
	- Relocate (hypervisor-level lift and shift): This migration scenario is specific to VMware Cloud on AWS, which supports VM workload portability between your on-premises environment and AWS.
	- Replatform (lift and reshape): Move an application to the cloud by taking advantage of cloud capabilities like PaaS. For example, migrate your on-premises Oracle database to Amazon RDS for Oracle.
	- Refactor (re-architect) – Move an application and modify its architecture by taking full advantage of cloud-native features to improve agility, performance, and scalability.
	- Repurchase (drop and shop) – Change to a different product, typically by moving from a traditional application to a software as a service (SaaS) product.
	- Retain (revisit) – Keep applications in your source environment, at least temporarily.
	- Retire – Decommission or remove applications that are no longer needed in your source environment.


# Server/Application Migration Options:


AWS Application Discovery Service:
- Helps you plan your migration to the AWS cloud by collecting usage and configuration data about your on-premises servers.
- Integrated with AWS Migration Hub.
- You can view the discovered servers, group them into applications, and then track the migration status of each application from the Migration Hub console
- You can export the system performance and utilization data for your discovered servers into your cost model to compute the cost of running those servers in AWS. 
- You can export data about the network connections that exist between servers. This information helps you determine the network dependencies between servers and group them into applications for migration planning. 
- Agentless discovery: can be performed by deploying the AWS Agentless Discovery Connector (OVA file) through your VMware vCenter.
- Agent-based discovery can be performed by deploying the AWS Application Discovery Agent on each of your VMs and physical servers. 


# AWS Server Migration Service (SMS):
- An agentless service that automates the migration of your on-premises VMware vSphere, Microsoft Hyper-V/SCVMM, and Azure virtual machines to the AWS Cloud. 
- Uses the Server Migration Service Connector which is a FreeBSD VM that you install in your on-premises virtualization environment. 
- Incrementally replicates your server VMs (snapshots) as cloud-hosted AMIs ready for deployment on Amazon EC2. 
- You can schedule replications and track progress of a group of servers that constitutes an application. 
- Supports Windows and Linux.
- Limit: 50 concurrent VM migrations per account.


# CloudEndure Migration:
- An agent-based tool that rehosts your applications on AWS.
- Supports self-service, highly automated, lift-and-shift migrations with minimal business disruption. 
- You install the CloudEndure Agent on your source machines.
- The Agent replicates your applications and data in a staging area on AWS.
- After the initial replication, the CloudEndure Agent tracks and migrates changes from your source environment to the target staging area by using asynchronous, block-level data replication, without causing downtime or affecting performance.

