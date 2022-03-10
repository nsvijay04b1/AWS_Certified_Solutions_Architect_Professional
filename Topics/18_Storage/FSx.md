# Amazon FSx for Windows File Server:
- Provides fully managed SMB file storage.
- Built on Windows Server.
- Options of HDD or SSD.
- Data deduplication.
- End-user file restore.
- Microsoft Active Directory (AD) integration.
- Single-AZ and multi-AZ deployment options.
- Standby file server in a separate AZ for single-AZ deployment.
- Automatic daily backups are turned on by default. Uses VSS service for consistency.
- Encryption of data at rest and in transit.


# Amazon FSx for Lustre:
- The open-source Lustre file system is designed for applications that require fast storage.
- You use Lustre for workloads where speed matters, such as machine learning, high performance computing (HPC), video processing, and financial modeling. 
- Provides sub-millisecond latencies, up to hundreds of GBps of throughput, and up to millions of IOPS.
- POSIX-compliant.
- Read-after-write consistency.
- Supports file locking. 
- Offers a choice of scratch and persistent file systems.
- Scratch file systems:
	- Ideal for temporary storage and shorter-term processing of data. 
	- Data is not replicated and does not persist if a file server fails. 
- Persistent file systems:
	- Ideal for longer-term storage and workloads. 
	- Data is replicated, and file servers are replaced if they fail. 
- Choice of SSD and HDD storage. In case of HDD, you can have an additional 20% SSD cache option.
- Integrated with S3: transparently presents S3 objects as files. 
- Can be used by Linux EC2 instances (using an open-source client) and EKS containers (using a driver).
- Supports encryption at rest and in transit using AWS KMS.

