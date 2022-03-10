# Amazon Aurora:
- A MySQL and PostgreSQL-compatible relational database built for the cloud.
- With some workloads, Aurora can deliver up to 5x the throughput of MySQL and up to 3x the throughput of PostgreSQL.
- The underlying storage grows automatically as needed. 
- Automatic clustering, replication, and storage allocation.
- An Aurora Cluster is composed of :
	- DB instances: up to 16.
	- A cluster volume that manages the data for those DB instances.
- Aurora cluster volume:
	- A storage volume that spans multiple AZs, with each AZ having 2 copies of the DB cluster data.
	- Each data chunk (10GB) is written in 6 copies: 3 AZs, 2 copies per AZ.
	- Supports loss of 2 copies.
	- Based on SSD disks.
	- Automatically grows and shrinks as the amount of data in your database increases or lowers.
	- Can grow to a maximum size of 128 tebibytes (TiB).
	- You are only charged for the space that you use in an Aurora cluster volume.
-Two types of DB instances make up an Aurora DB cluster: 
	- Primary DB instance: Read and Write. One per cluster. 
	- Aurora Replica: Read only. Up to 15 per cluster.
- Replicas can be placed in other AZs.
- Aurora automatically fails over to an Aurora Replica.
- Aurora multi-master clusters: all DB instances have read/write capability.
- Clusters are created in a VPC. Security Groups can be used.
- Storage can be encrypted.

## Aurora Scaling:
- Performance scaling: manual or automatic with read replicas.
- Storage scaling: automatic.

## Aurora DB endpoints:
- An endpoint is represented as an Aurora-specific URL that contains a host address and a port. 
- Cluster endpoint (or writer endpoint):
	- Connects to the current primary DB instance for that DB cluster. 
	- This endpoint is the only one that can perform write operations such as DDL statements. 
	- If the current primary DB instance of a DB cluster fails, Aurora automatically fails over to a new primary DB instance. 
- Reader endpoint:
	- Provides read-only connections to the DB cluster on the read-only Aurora Replicas.
	- If the cluster contains multiple Aurora Replicas, the reader endpoint load-balances each connection request among the Aurora Replicas.
	- If the cluster contains only a primary instance and no Aurora Replicas, the reader endpoint connects to the primary instance. 
- Custom endpoint:
	- Represents a set of DB instances that you choose. 
	- When you connect to the endpoint, Aurora performs load balancing and chooses one of the instances in the group to handle the connection. 
	- You can create up to 5 custom endpoints.
- Instance endpoint:
	- Connects to a specific DB instance within an Aurora cluster.
	- Each DB instance in a DB cluster has its own unique instance endpoint.

## Aurora Replication options:
- Different regions: using the Aurora Global Database feature (Up to 5 replicas).
- Same region: using MySQL binary log (binlog) replication (Up to 5 replicas) or using PostgreSQL's logical replication.

## Aurora Global Database:
- A feature that allows a single Amazon Aurora database to span multiple AWS regions.
- Composed of a primary DB cluster in one Region, and up to 5 read-only secondary DB clusters in different Regions.
- Enables fast local reads in each region.
- Asynchronous replication. Replication latency: 1 sec (RPO = 1s).
- Manual failover: a secondary region can be promoted to full read/write capabilities in less than 1 minute (RTO = 1mn).

## Aurora database authentication options:
- Password authentication: internal DB users. The DB admin creates users with SQL statements.
- IAM database authentication: uses standard AWS IAM authentication. An authentication token is used to connect to the DB. 
- Kerberos authentication: Kerberos and Microsoft AD authentication and SSO.

## Aurora Serverless:
- An on-demand, autoscaling configuration for Aurora.
- Automatically starts up, shuts down, and scales capacity up or down based on your application's needs.
- Advantages: Similar than provisioned Aurora, Scalable, Cost-effective.
- Does not support Global databases, multi-master, replicas, …