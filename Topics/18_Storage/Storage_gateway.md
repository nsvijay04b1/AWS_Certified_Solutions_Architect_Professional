# AWS Storage Gateway:
- Appliance that offers file-based, volume-based, and tape-based storage solutions on prem.

## File Gateway:
- A file interface into Amazon S3.
- Software appliance. Supports ESXi, Hyper-V and KVM.
- Provides file access with NFSv4 and SMB.

## Volume Gateway:
- S3-backed storage volumes that you can mount as iSCSI devices.
- Software Appliance. Supports ESXi, Hyper-V and KVM.
- Two volume types:
	- Cached Volumes: Cloud S3 volumes that are cached locally on the appliance. Up to 32 TB per volume.
	- Stored Volumes: Can store volumes locally on the appliance with snapshots to S3 (as EBS snapshots). Up to 16 TiB per volume.

## Tape Gateway:
- Cloud-backed VTL services
- Can run on-premises as a VM appliance, as a hardware appliance, or in AWS as an Amazon EC2 instance (for apps hosted on EC2). 
- Supports ESXi, Hyper-V and KVM.
- Virtual Tapes are backed by S3 with a local cache and buffer on the appliance.
- Virtual Tape Shelf (VTS) backed by S3 with choice of storage classes: GLACIER or DEEP_ARCHIVE.