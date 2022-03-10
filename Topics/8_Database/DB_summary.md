# AWS Database features summary:
In Region Data availability:
	- RDS: 		multi-AZ = 2 synchronous copies and 2 instances in 2 AZs. Automatic Failover.
	- Aurora : 	6 synchronous copies and up to 16 instances in 3 AZs. Automatic Failover.
	- DynamoDB: 	3 synchronous copies in 3 AZs. Automatic Failover.
Cross-Region Data availability:
	- RDS: 		Read Replicas. Up to 5 Asynchronous Read Replicas. Manual Failover.
	- Aurora: 	Global Database = Up to 5 Asynchronous read-only secondary DB clusters in different Regions. RPO = 1 sec. RTO = 1mn. Manual Failover.
	- DynamoDB: 	Global Tables = multiple read/write replica tables (one per Region). Eventually consistent. RPO = 1 sec. Automatic Failover can be implemented in the application logic.
Authentication:
	- RDS: 		SQL Password, AWS IAM (MySQL & PostgreSQL), Kerberos.
	- Aurora: 	SQL Password, AWS IAM, Kerberos.
	- DynamoDB: 	AWS IAM.
Notifications:
	- RDS: 		SNS.
	- Aurora: 	SNS.
	- DynamoDB: 	Kinesis Data Streams or Lambda triggers.

Performance: RDS vs DynamoDB:
- DynamoDB can handle higher rates for read/write operations (10K+ operations per second).
- DynamoDB can scale with sharding. RDS has a single node/shard.
