# AWS DataSync:
- An online data transfer service between on-premises storage systems and AWS storage services, and also between AWS storage services. 
- DataSync can copy data between NFS, SMB file servers, self-managed object storage, AWS Snowcone, S3 buckets, EFS file systems, and Amazon FSx file systems. 
- Includes automatic encryption.
- Can Move cold data stored in on-premises storage directly to durable and secure long-term storage such as Amazon S3 Glacier or S3 Glacier Deep Archive. 
- A DataSync agent is a VM that you own that is used to read or write data from self-managed storage systems. 
- A location is an endpoint of a task. Each task has two locations—a source location and a destination location. 
- You can narrow down the copied files by filtering the data or by configuring DataSync to not overwrite files that are already present on the destination. 
- DataSync verifies data integrity: locally calculates the checksum of every file in the source file system and the destination and compares them.
- It is a data transfer service which allows data to be transferred into or out of AWS
- Can be used for workflows such as migrations, data processing transfers, archival, cost effective storage, DR/BC
- Each agent can handle 10Gbps transfer speed, each job can handle 15 million files
- It also handles the transfer of metadata (permissions, timestamps)
- It provides built in data validation

## Key Features

- Scalable: 10Gbps per agent (~100TB of data per day)
- Bandwidth Limiters: used to avoid link saturation
- Incremental and scheduled transfer options
- Compressions and encryption
- Automatic recovery from transit errors
- Service integration: S3, EFS, FSx, service-to-service transfer
- Pay as you use service: per GB of data transferred
- The DataSync agent runs on a virtualization platform such as VMWare

## DataSync Components

- Task: a job within DataSync, defines what is being synced, how quickly, from where and to where
- Agent: software used to read or write to on-premises data stores using NFS or SMB
- Location: every task has two locations from and to, examples: Network File System (NFS), Server Message Block (SMB), Amazon EFS, Amazon FSx and Amazon S3
