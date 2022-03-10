# Elastic Bloc Storage (EBS):
- Provides storage volumes that are seen as iSCSI targets.
- EBS volumes can be attached to your EC2 instances and seen as disks by the OS.
- Does not need to be attached to an instance,
- Cannot be attached to more than one instance,
- Can be transferred between AZs.
- EBS currently supports a maximum volume size of 16 TiB.
- Volume size can be increased while attached.
- Volumes are replicated in a single AZ,
- Encryption supported with AWS KMS.
- Snapshots supported.

## EBS Optimized Instances:
- EBS-optimized instances are EC2 instances that have dedicated bandwidth to EBS.
- Designed for use with all EBS volume types
- not all EC2 instance types support EBS Optimization.
- some EC2 instance types have EBS Optimization by default.
- Up to 150 Gbps of bandwidth for certain instance types.
- Additional hourly fee.

## EBS Snapshots:
- You can create point-in-time snapshots of EBS volumes, which are persisted to Amazon S3 (but you don’t see them in your S3 bucket).
- You can make a snapshot of an EBS volume or an EBS-backed EC2 instance.
- The first snapshot is a complete copy of the volume to S3.
- Subsequent snapshots are incremental: only the blocks on the device that have changed after your most recent snapshot are saved. 
- When you delete a snapshot, only the data unique to that snapshot is removed. If you delete a snapshot containing data being used by a later snapshot, the referenced data is preserved. 
- When creating a volume from a Snapshot, you can resize it.
- Can be shared.
- Can be copied across regions.
- Encryption:
	- Snapshots of encrypted volumes are automatically encrypted.
	- Volumes created from encrypted snapshots are automatically encrypted.
	- Volumes created from unencrypted snapshots can be encrypted on the fly.
	- When you copy an unencrypted snapshot, you can encrypt it during the copy process.
	- You can re-encrypt a snapshot with a different key.

## EBS Volume Types:
- Provisioned IOPS (io1, io2, io2BE):
	- SSD
	- IOPS: 256K for io2BE, 64K for io1/io2. 64K is only achievable on Nitro instances. Other instances are capped to 32K IOPS. 
	- 4,000 MB/s for io2BE, 1,000 MB/s for io1/io2
	- You can provision from 100 IOPS up to 64,000 IOPS per volume on Nitro-based Instances and up to 32,000 on other instances. 
	- maximum ratio of provisioned IOPS to volume size (in GiB) is 50:1 for io1 volumes, and 500:1 for io2 volumes. 
- General Purpose (gp2, gp3):
	- SSD
	- 16K IOPS.
	- 1,000 MB/s for gp3 and 250 MB/s for gp2.
	- The performance of gp2 volumes is tied to volume size, which determines the baseline performance level of the volume and how quickly it accumulates I/O credits; larger volumes have higher baseline performance levels and accumulate I/O credits faster.
- Throughput Optimized HDD (st1):
	- HDD
	- 500 IOPS
	- 500 MB/s
	- Cannot be used as boot drive
- Cold HDD (sc1):
	- HDD
	- 250 IOPS
	- 250 MB/s
	- Cannot be used as boot drive
- Magnetic:
	- Previous generation HDD.
	- 40-200 IOPS
	- 90 MB/s
