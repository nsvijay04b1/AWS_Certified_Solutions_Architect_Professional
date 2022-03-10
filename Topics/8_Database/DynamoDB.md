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

## DynamoDB Operation, Consistency and Performance

- We can chose between to different capacity mode at table creation: on-demand and provisioned
- We may be able to switch between this capacity mode afterwards
- On-demand capacity mode: 
    - Designed for unknown, unpredictable load
    - Requires low administration
    - We don't have to explicitly set capacity settings, all handled by DynamoDB
    - We pay a price per million R or W unit
- Provisioned capacity mode:
    - We set the RCU/WCU per table
- Every operation consumes at least 1RCU/WCU
- 1 RCU is `1 * 4KB` read operation per second for strongly consistent reads, `2 * 4KB` read operations per second for eventual consistent reads
- 1 WCU is `1 * 1KB` write operation per second
- Every table has a RCU and WCU bust pool (300 seconds)
- DynamoDB operations:
    - **Query**:
        - When a query is performed we need to take a partition key
        - Query item can return 0 or more items, but we have to specify the partition key every time
        - We can specify specific attribute we would want to be returned, we will be charged for querying the whole item anyway
    - **Scan**:
        - Least efficient operation, but the most flexible
        - Scan moves through a table consuming the capacity of every item
        - Any attribute can be used and any filters can be applied, but scan will consume the capacity for every item scanned through
- DynamoDB can operate using two different consistency modes:
    - Eventually consistent
    - Strongly consistent
- DynamoDB replicates data cross AZs using storage nodes. Storage nodes have a leader node, which is elected from the existing nodes
- Writes are directed to leader node
- The leader nodes replicates data to other nodes, typically finishing within a few milliseconds

## WCU/RCU Calculation

- Example: we need to store 10 items per second, 2.5K average size per item
    - WCU required: 
        ```
        ROUND UP(ITEM SIZE / 1 KB) => 3
        MULT by average (30) => WCU required = 30
        ```
- Example: we need to retrieve 10 items per second, 2.5K average size per item
    - RCU required:
        ```
        ROUND UP (ITEM SIZE / 4 KB) => 1
        MULT by average read ops per second (10) => Strongly consistent reads = 10, Eventually consistent reads => 5

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

## DynamoDB Indexes

- Are way to improve efficiency of data retrieval from DynamoDB
- Indexes are alternative views on table data, allowing the query operation to work in ways that it couldn't otherwise
- Local Secondary Indexes allow to create a view using different sort key, Global Secondary Indexes allow to create create different partition and sort key

### Local Secondary Indexes (LSI)

- **Must be created with the base table!**
- We can have at max 5 LSIs per base table
- LSIs are alternative sort key, same partition key
- They share the same RCU and WCU with the main table
- Attributes which can be projected into LSIs: `ALL`, `KEYS_ONLY`, `INCLUDE` (we can specifically pick which attribute to be included)
- Indexes are sparse: only items which have a value in the index alternative sort key are added to the index

### Global Secondary Indexes (GSI)

- They can be created at any time
- It is a default limit of 20 GSIs per base table
- We cane define different partition and sort keys
- GSIs have their own RCU and WCU allocations
- Attributes projected into the index: `ALL`, `KEYS_ONLY`, `INCLUDE`
- GSIs are also sparce, only items which have values in the new PK and optional SK are added to the index
- GSIs are always eventually consistent, the data is replicated from the main table

### LSI and GSI Considerations

- We have to be careful with the projection, more capacity is consumed if we project unnecessary attributes
- If we don't project a specific attribute and require that when querying the index, it will fetch the data from the main table, the query becoming inefficient
- AWS recommends using GSIs as default, LSI only when strong consistency is required


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
- DAX operates within a VPC, designed to be deployed in multiple AZs in a VPC
- DAX is a cluster service, nodes are placed in different AZs. There a primary nodes from which data is replicated into replica nodes
- DAX maintains 2 different caches:
    - Items cache: holds results of (`Batch`)`GetItem` calls
    - Query cache: holds the collection of items based on query/scan parameters
- DAX is accessed via an endpoint. This endpoint load balances across nodes
- Cache hits are returned in microseconds, cache misses in milliseconds
- When writing data to DynamoDB, DAX uses write-through caching, the data is written at the same time to the cache as it is written to the DB
- DAX is not suitable for applications requiring strongly consistent reads

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

- A stream is a time ordered list of item changes in a DynamoDB table
- A stream is a 24H rolling window
- Streams has to be enabled per table basis
- Streams record inserts, updates and deletes
- We can create different view types influencing what is in the stream
- Available view types:
    - `KEYS_ONLY`: the stream will only record the partition key and available sort keys for items which did change
    - `NEW_IMAGE`: stores the entire item with the new state after the change
    - `OLD_IMAGE`: stores the entire state of the item before the change
    - `NEW_AND_OLD_IMAGE`: stores the before/after states of the items in case of a change
- In some cases the new/old states recorded can be empty, example in case of a deletion the new state of an item is blank
- Streams are the foundation for database triggers
- An item change inside a table generate an event, which contains the data which changed
- An action is taken using that data in RDSIAMAuthentication of event
- We can use streams and Lambda in case of changes and events
- Streams and triggers are useful for data aggregation, messaging, notifications, etc.

## DynamoDB Backups


- Point-in-time Recovery:
    - Not enabled by default, has to be enabled
    - It is a continuous record of changes
    - Allows replay to any point in the window (35 days recovery window)
    - From this 35 day window we can restore to another table with a 1 second granularity

Two types of backups:
- on-demand backup: 
	- full backups of your tables for long-term archival.
	- All on-demand backups are cataloged, discoverable, and retained until explicitly deleted. 
	- Can be scheduled using AWS Backup.
    - We can retain or remove indexes
    - We can adjust encryption settings
- continuous automatic backups(Point-in-time Recovery): 
    - Not enabled by default, has to be enabled
    - It is a continuous record of changes
    - Allows replay to any point in the window (35 days recovery window)
    - From this 35 day window we can restore to another table with a 1 second granularity
	- enables Point-in-time Restore (PITR),
	- you can restore to any time between 5 minutes and 35 days.
Both types support cross-region restores.

### NoSQL Workbench:
- A client-side GUI application for NoSQL database development and operations.
- A unified visual IDE tool that provides data modeling, data visualization, and query development features to help you design, create, query, and manage DynamoDB tables.
- Available for Windows, macOS, and Linux. 