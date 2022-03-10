# AWS DataSync:
- An online data transfer service between on-premises storage systems and AWS storage services, and also between AWS storage services. 
- DataSync can copy data between NFS, SMB file servers, self-managed object storage, AWS Snowcone, S3 buckets, EFS file systems, and Amazon FSx file systems. 
- Includes automatic encryption.
- Can Move cold data stored in on-premises storage directly to durable and secure long-term storage such as Amazon S3 Glacier or S3 Glacier Deep Archive. 
- A DataSync agent is a VM that you own that is used to read or write data from self-managed storage systems. 
- A location is an endpoint of a task. Each task has two locations—a source location and a destination location. 
- You can narrow down the copied files by filtering the data or by configuring DataSync to not overwrite files that are already present on the destination. 
- DataSync verifies data integrity: locally calculates the checksum of every file in the source file system and the destination and compares them.
