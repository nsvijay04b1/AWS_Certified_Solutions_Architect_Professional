# Amazon Neptune:
- A fully managed graph database service.
- Neptune is highly available, with read replicas, point-in-time recovery, continuous backup to Amazon S3, and replication across Availability Zones. 
- Neptune provides data security features, with support for encryption at rest and in transit. 
- Supports open graph APIs for both Gremlin and SPARQL.
- Key Service Components
	- Cluster volume: Neptune data is stored in the cluster volume, which consists of copies of the data across multiple Availability Zones in a single AWS Region. 
	- Primary DB instance: Supports read and write operations, and performs all of the data modifications to the cluster volume. One per cluster.
	- Neptune replica: Connects to the same storage volume as the primary DB instance and supports only read operations. Up to 15 Replicas per cluster.
- Supports IAM database authentication.