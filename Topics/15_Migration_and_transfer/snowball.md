# AWS Snowball:
- an edge computing, data migration, and edge storage device.
- devices provide both block storage and Amazon S3-compatible object storage. NFS also supported for S3.
- Data is encrypted (256-bit).
- comes in two options:
	- Snowball Edge Storage Optimized: 40 vCPUs, 80TB. Suited for local storage and large scale-data transfer. 
	- Snowball Edge Compute Optimized: 52 vCPUs, 42TB, optional GPU. Use cases like advanced machine learning and full motion video analysis in disconnected environments. 
- Supports specific Amazon EC2 instance types and AWS Lambda functions.
- Can be clustered.
- Comes in shippable, hardened, secure cases.
- Can be rack mounted.
- Weight: 22 kg.


# AWS Snowcone:
- portable, rugged, and secure device for data storage, data transfer and edge computing.
- can run edge computing workloads that use AWS IoT Greengrass or Amazon EC2 instances. 
- You can use Snowcone to collect, process, and move data to AWS, either offline by shipping the device, or online with AWS DataSync.
- Usable Storage: 8 TB.
- Wifi
- Battery based.
- Weight: 2 kg.


# AWS Snowmobile:
- 45-foot long ruggedized shipping container.
- appears as a network-attached data store (NFS): 6x 40 GE connectivity.
- Can ingest up to 1 Tb/s of data.
- Usable Storage: 100 PB.
- Tamper-resistant, waterproof, and temperature controlled.
- Multiple layers of logical and physical security.
- Data is encrypted (256-bit).
- Two power options: local utility power source or another container hosting a generator set.