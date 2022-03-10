# Amazon DynamoDB
- Complete PaaS NoSQL DB with high scalability and low latency.
- Automatically and synchronously replicates data across 3 AZs.
- Uses SSDs.
- You can scale up or down without impact.
- ElastiCache can be used in front of DynamoDB.
- Not ideal for Joins and complex transactions.
- Schema-less DB
- Stores structured data in tables, indexed by a primary key.
- Data structure:
	- a table is a collection of items. 
	- Each item is a collection of attributes. 
	- Each attribute has a name and a value. 
	- One (or two) of the attributes is a unique identifier of the items and is called the primary key.
- Primary key can be 
	- a single attribute. In this case: Primary Key = Partition Key.
	- a combination of two attributes (composite key):
		- The first attribute is called the Partition Key (or Hash Attribute).
		- The second attribute is called the Sort Key (or the Range Attribute).
- All items with the same partition key value are stored together (physical storage), in sorted order by sort key value. 
- Some attributes can be scalar (one value) or nested (recursive dictionary up to 32 levels deep).
###  Search types: Query and Scan.
- Query: 
	- Finds items using the primary key.
	- Optionally, you can provide a sort key to refine the search.
- Scan: 
	- Reads every item in a table or a secondary index. 
	- By default, a Scan operation returns all of the data attributes for every item.
	- You can use the ProjectionExpression parameter so that Scan only returns some of the attributes, rather than all of them.
	- Can retrieve a maximum of 1 MB of data. Uses pagination if the scan result is above 1 MB.
	- A filter can be applied to the scan but it is applied after retrieval.
- Supports Cross-Region Replication (see "Global Tables" below).
- Supports automatic deletion of expired items.

### DynamoDB Secondary Indexes:
- A data structure that contains a subset of attributes from a table, along with an alternate key to support Query operations. 
- When you create a secondary index, you define an alternate key for the index (partition key and sort key) and the attributes that you want to be projected (copied) from the base table into the index. 
- Data is automatically maintained by DynamoDB upon change in the base table.
- You can create one or more secondary indexes on a table. 
- Can be single or composite (partition key and sort key).
- Support also Scan operations. 
- Two types of secondary indexes:
	- Global: with a partition key and sort key that can be different from those on the table. It is considered "global" because queries on the index can span all of the data in the base table, across all partitions. 
	- Local:has the same partition key as the table, but a different sort key. It is considered "local" in the sense that every partition of a local secondary index is scoped to a base table partition that has the same partition key value. 

### DynamoDB Partitions and Data Distribution:
- DynamoDB stores data in partitions. 
- A partition is an allocation of storage for a table, backed by solid state drives (SSDs) and automatically replicated across multiple AZs.
- When you create a table, DynamoDB allocates sufficient partitions to the table so that it can handle your provisioned throughput requirements.
- DynamoDB allocates additional partitions to a table in the following situations:
	- If you increase the table's provisioned throughput settings beyond what the existing partitions can support.
	- If an existing partition fills to capacity and more storage space is required.
- Partition management occurs automatically in the background and is transparent to your applications.  
- To write an item to the table, DynamoDB uses the value of the partition key as input to an internal hash function. The output value from the hash function determines the partition in which the item will be stored. 

### DynamoDB Capacity modes:
- Amazon DynamoDB has two read/write capacity modes for processing reads and writes on your tables:
	- On-demand: pay-per-request.
	- Provisioned: default.
- The mode can be changed.
- Provisioned mode : 
	- free-tier eligible,
	- puts limits for cost predictability,
	- supports auto scaling
	- supports reserved capacity.
- Capacity Units:
	- You specify throughput requirements in terms of capacity units.
	- A read capacity unit (RCU) represents one strongly consistent read per second, or two eventually consistent reads per second, for an item up to 4 KB in size. 
	- A write capacity unit (WCU) represents one write per second, for an item up to 1 KB in size. 

### DynamoDB Auto Scaling:
- Uses the AWS Application Auto Scaling service to dynamically adjust provisioned throughput capacity on your behalf, in response to actual traffic patterns.
- Enables a table or a global secondary index to increase its provisioned read and write capacity to handle sudden increases in traffic, without throttling. 
- When the workload decreases, Application Auto Scaling decreases the provisioned throughput so that you don't pay for unused provisioned capacity. 
- You create a scaling policy for a table or a global secondary index.
- The scaling policy specifies whether you want to scale read capacity or write capacity (or both), and the minimum and maximum provisioned capacity unit settings for the table or index.
- The scaling policy also contains a target utilization:
	- Target utilization = the percentage of consumed throughput vs provisioned throughput. 
	- Adjusts the provisioned throughput of the table (or index) upward or downward in response to actual workloads, so that the actual capacity utilization remains at or near your target utilization. 
	- The target utilization can be set between 20% and 90%.

### DynamoDB Accelerator (DAX):
- A managed in-memory cache for DynamoDB.
- Reduces the response times of eventually consistent read workloads by an order of magnitude from single-digit milliseconds to microseconds.
- API-compatible with DynamoDB: requires minimal changes to use with an existing application.
- Ideal for applications that need very fast or intensive reads on all or a subset of the data.
- NOT ideal for applications that require strongly consistent reads or apps that are write-intensive.
- A DAX cluster consists of one or more nodes in your VPC. 
- Each node runs its own instance of the DAX caching software. 
- One of the nodes serves as the primary node for the cluster. 
- Additional nodes (if present) serve as read replicas.

### DynamoDB Global Tables:
- Provide a fully managed solution for deploying a multiregion, multi-active database.
- A DynamoDB global table consists of multiple replica tables (one per Region) that DynamoDB treats as a single unit.
- All replicas are Read/Write.
- Newly written items are usually propagated to all replica tables within a second. 
- When doing a read, your application can choose between "eventually consistent" (default) and "strongly consistent" reads. 
- If your application requires strongly consistent reads, it must perform all of its strongly consistent reads and writes in the same Region. DynamoDB does not support strongly consistent reads across Regions. 
- If applications update the same item in different regions, the last writer wins.
- Failover: You can apply custom business logic to determine when to redirect requests to other Regions. 

### DynamoDB Transactions:
- You can group multiple actions together and submit them as a single all-or-nothing.
- Idempotency: You can optionally include a client token when you make a TransactWriteItems call to ensure that the request is idempotent.
- Making your transactions idempotent helps prevent application errors if the same operation is submitted multiple times due to a connection time-out or other connectivity issue. 
- Transactions in Global Tables:
	- Provide atomicity, consistency, isolation, and durability (ACID) guarantees only within the region where the write is made originally.
	- Are not supported across regions in global tables.

### DynamoDB Conditional Writes:
- By default, the DynamoDB write operations (PutItem, UpdateItem, DeleteItem) are unconditional: Each operation overwrites an existing item that has the specified primary key. 
- When you have multiple users attempt to modify the same item (for example: incrementing a counter), you want your write operation to succeed only if the item attributes still have the expected value that your user application read before from the Database.
- DynamoDB supports conditional writes for these write operations: a conditional write succeeds only if the item attributes meet one or more expected conditions. 

### DynamoDB Access:
- Based on AWS IAM.
- You need to use permissions: user inline policy, user, group or role with attached policy.
- Using a role is preferred as the credentials you receive when you assume a role are time-bound.
- With roles, you can grant cross-account permissions.
- DynamoDB does not support resource-based policies.

### DynamoDB Encryption At-Rest:
- Supports Encryption at rest using KMS: CMKs can be AWS-owned, AWS-managed or Customer-managed.
- Supports also encryption using the DynamoDB Encryption Client (software library). Can be used to:
	- Encrypt some attributes in a table.
	- Sign elements in a table to prevent alteration. The signature is added to the element as an attribute.

### DynamoDB Streams: 
- captures a time-ordered sequence of item-level modifications in any DynamoDB table and stores this information in a log for up to 24 hours. 
- Applications can access this log and view the data items as they appeared before and after they were modified, in near-real time. 
- AWS maintains separate endpoints for DynamoDB and DynamoDB Streams.
- Stream records are organized into groups, or shards.
- Can be consumed either by:
	- AWS Kinesis (DynamoDB Streams Kinesis Adapter). 
	- Lamda trigger: AWS Lambda polls the stream and invokes your Lambda function synchronously when it detects new stream records. 

### DynamoDB Backup:
Two types of backups:
- on-demand backup: 
	- full backups of your tables for long-term archival.
	- All on-demand backups are cataloged, discoverable, and retained until explicitly deleted. 
	- Can be scheduled using AWS Backup.
- continuous automatic backups: 
	- enables Point-in-time Restore (PITR),
	- you can restore to any time between 5 minutes and 35 days.
Both types support cross-region restores.

### NoSQL Workbench:
- A client-side GUI application for NoSQL database development and operations.
- A unified visual IDE tool that provides data modeling, data visualization, and query development features to help you design, create, query, and manage DynamoDB tables.
- Available for Windows, macOS, and Linux. 