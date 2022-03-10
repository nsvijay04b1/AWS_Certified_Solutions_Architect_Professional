# AWS Backup:
- A fully managed backup service that makes it easy to centralize and automate the backup of data across AWS services in the cloud and on premises.
- You can create backup policies that automate backup schedules and retention management. 
- Supported AWS resources: EFS, EBS, EC2, FSx, DynamoDB, RDS, Aurora, Storage Gateway.
- You can create backup policies known as backup plans. 
- You can apply backup plans to your AWS resources by tagging them. 
- You can copy backups to multiple AWS Regions on demand or automatically as part of a scheduled backup plan. 
- A backup vault is a container that you organize your backups in. 


# Amazon Data Lifecycle Manager (DLM):
- Automate the creation, retention, and deletion of EBS snapshots and EBS-backed AMIs. 
- Uses resource tags to identify the resources to back up. 
- Policy schedules define when snapshots or AMIs are created by the policy. 
- Policies can have up to four schedules—one mandatory schedule, and up to three optional schedules. 
- Retention: you can retain snapshots or AMIs based either on their total count (count-based), or their age (age-based). 


# AWS Backup vs AWS DLM:
- You should use DLM when you want to automate the creation, retention, and deletion of EBS snapshots. 
- You should use AWS Backup to manage and monitor backups across the AWS services you use, including EBS volumes, from a single place.