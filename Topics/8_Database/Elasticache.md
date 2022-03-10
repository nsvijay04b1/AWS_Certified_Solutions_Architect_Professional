# Amazon ElastiCache:
- Managed In-memory caching service.
- Choice of engines: Memcached or Redis.

## Memcached:
- simple features, 
- multithreaded (sharding).
- low maintenance,
- easy horizontal scaling with Auto Discovery.

## Redis:
- Has much more features than Memcached.
- Nodes:
	- A node is the smallest building block of an ElastiCache deployment. 
	- Multiple types of cache nodes are supported (EC2 instance types).
	- You can purchase nodes on a pay-as-you-go basis, or you can purchase reserved nodes at a much-reduced hourly rate.
- A Redis shard:
	- Called also a "node group" or "replication group".
	- Is a grouping of 1 read/write primary node and up to 5 read-only replica nodes. 
	- Read replicas use asynchronous replication. 
	- A node group supports Multi-AZ: locating read replicas in multiple AZs with Automatic failover.
- Redis clusters:
	- A Redis (cluster mode disabled) cluster always has one shard. You can have a "write endpoint" and a "read endpoint".
	- A Redis (cluster mode enabled) cluster can have 1–250 shards. Has a single endpoint called "configuration endpoint" that can provide to your application the primary and read endpoints for each shard in the cluster. 
	- Every node within a cluster is the same instance type and runs the same cache engine. 
- Supports Backup/Restore, snapshots.
- Supports Transactions,
- Supports Publish/subscribe,
- Supports sorting/ranking, advanced data types,
- Caching strategies include:
	- Lazy loading: populate cache whenever data is read from the database. Pros: small cache size. Cons: latency on cache miss.
	- Write-through strategy: adds data or updates data in the cache whenever data is written to the database. Pros: good read latency. Cons: cache size can get big, double write latency.
- Redis data durability options:
	- Daily automatic backups.
	- Manual backups using Redis append-only file (AOF, see below).
	- Setting up a Multi-AZ with Automatic Failover.
- Data persistance:
	- By default, the data in a Redis node resides only in memory and isn't persistent. 
	- If you require data persistence, you can enable the Redis append-only file feature (AOF).
	- When AOF is enabled, the node writes all of the commands that change cache data to an append-only file.
	- When a node is rebooted and the cache engine starts, the AOF is "replayed." The result is a warm Redis cache with all of the data intact. 
- Using Redis AUTH command can improve data security by requiring the user to enter a password before they are granted permission to execute Redis commands on a password-protected Redis server.
- Supports Security Groups.
- Supports IAM access security.