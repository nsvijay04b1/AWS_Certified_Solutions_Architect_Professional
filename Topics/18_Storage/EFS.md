# Elastic File System (EFS):
- Simple, petabyte scalable NFS file storage for EC2 instances.
- Protocol: NFS v4.
- Elastic: automatically grows and strings as you add or remove files.
- Stored redundantly across multiple AZs,
- Can be accessed concurrently by 1000s of EC2 instances from different AZs.
- On-premise supported via direct connect.

## Storage classes:
### Standard storage class: 
- for frequently accessed files. 
- Single-digit latencies on average.
### Infrequent Access (IA) storage class 
- A lower-cost storage class that's designed for storing long-lived, infrequently accessed files cost-effectively.
- Double-digit latencies on average.

## Mount Targets:
- You mount your file system on an EC2 instance using a mount target that you create for the file system. 
- An EFS file system can only be attached to one single VPC at a time.
- You can create only one mount target per Availability Zone.

## Lifecycle management: 
- a lifecycle policy migrates files that have not been accessed for a set period of time to the Infrequent Access (IA) storage class. 
- Possible values for the period: 7, 14, 30, 60 and 90 days.
- Lifecycle policy applies to the whole filesystem.
- Locking in Amazon EFS follows the NFSv4.1 protocol for advisory locking, and enables your applications to use both whole file and byte range locks.

## EFS Performance Modes:
- EFS offers two performance modes, General Purpose mode and Max I/O mode. 
- General Purpose performance mode:
	- Default.
	- Recommended for the majority of use cases and latency-sensitive use cases.
- Max I/O performance mode: 
	- Can scale to higher levels of aggregate throughput and operations per second. 
	- This scaling is done with a tradeoff of slightly higher latencies for file metadata operations. 
	- Recommended for highly parallelized applications and workloads, such as big data analysis and media processing.

## EFS Throughput Modes:
- There are two throughput modes to choose from for your file system: Bursting Throughput and Provisioned Throughput. 
- Bursting Throughput mode:
	- Throughput scales as the size of your file system in the standard storage class grows. 
	- File systems of less than 1 TiB can burst to 100 MiB/s of metered throughput. 
	- File systems of more than 1 TiB can burst to 100 MiB/s of metered throughput per TiB of storage.
	- The portion of time that a file system can burst is determined by its size. 
	- EFS uses a credit system to determine when file systems can burst. 
	- A file system accumulates burst credits whenever it is inactive or driving throughput below its baseline metered rate (50 MiB/s per TiB per storage). 
- Provisioned Throughput mode:
	- You can instantly provision the throughput of your file system (in MiB/s) independent of the amount of data stored. 
- EFS file systems meter read requests at one-third the rate of other requests. 

## IAM authorization for NFS clients:
- This is an option you can activate.
- NFS clients can identify themselves using an IAM role when connecting to an EFS file system. 
- EFS resource-based policies are called file system policies.
- The default EFS file system policy grants full access to any client that can connect to the file system using a file system mount target. 
- Using a file system policy, you can specify the following actions for clients wanting to mount a file system.
	- ClientMount: Provides read-only access to a file system.
	- ClientWrite: Provides write permissions on a file system.
	- ClientRootAccess: Provides use of the root user when accessing a file system.
- Controlled conditions: TLS encryption, specific Access Points.

## EFS Access Points:
- EFS Access Points are entry points into an EFS file system that make it easier to manage application access to shared datasets. 
- When user enforcement is enabled, Amazon EFS replaces the NFS client's user and group IDs with the identity configured on the access point for all file system operations.
- Can also enforce a different root directory for the file system so that clients can only access data in the specified directory or its subdirectories. This is the "Path" attribute of the access point.
- You can use an IAM policy to enforce that a specific NFS client, identified by its IAM role, can only access a specific access point. To do this, you use the elasticfilesystem:AccessPointArn IAM condition key.

## EFS File-level permissions:
- The mount command can mount any directory in the file system. 
- By default only the root user (UID 0) has read, write, and execute permissions.
- When an NFS client mounts an EFS file system without using an access point, the user ID and group ID provided by the client is trusted. You can use EFS access points to override user ID and group IDs used by the NFS client. 
- EFS file access authorization is based on the UNIX-like numeric user and group identifiers provided by the NFS client.
- For non-root users to modify the file system, the root user must explicitly grant them access. 

## EFS Encryption:
- Encryption of data at rest (using KMS) and in transit are both supported.
- You cannot change an existing EFS to use encryption.
- You can use the elasticfilesystem:Encrypted condition key in IAM identity-based policies to control whether users can create encrypted/unencrypted EFS file systems.
- You can define SCPs inside AWS Organizations to enforce EFS encryption for all AWS accounts in your organization. 
- Data and metadata are automatically encrypted before being written to the file system.
- Metadata is encrypted using an AWS-managed CMK.
- File data is encrypted using an AWS-managed CMK or a customer-managed CMK. 