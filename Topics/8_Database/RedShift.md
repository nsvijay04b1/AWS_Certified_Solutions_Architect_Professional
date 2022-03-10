# Amazon RedShift:
- Fully managed petabyte-scale relational data warehouse service.
- HDD and SSD platforms.
- Wide integrations with existing tools.
- Architecture:
	- Leader Node: Simple SQL end point, stores metadata, coordinates query execution.
	- Compute Nodes: Local columnar storage, parallel execution of queries, loads, backups, restores and resizes.
	- You select the instance types for your nodes.
	- Lives inside a VPC.
- Redshift workload management (WLM): enables users to manage priorities within workloads so that short, fast-running queries won't get stuck in queues behind long-running queries. WLM assigns the query to a queue according to the user's group.
- Enhanced VPC Routing feature:
	- If not enabled, Redshift routes traffic through the internet, including traffic to other services within the AWS network. 
	- If enabled, Redshift forces all COPY and UNLOAD traffic between your cluster and your data repositories through your VPC.
	- If enabled, you can use features such as ACLs, VPC endpoints, internet gateway, DNS.
- Redshift Can load encrypted data from S3.
- Supports Encryption at Rest and in Transit (SSL).
- Supports VPC Security Groups.
- Snapshots:
	- Manual or Automated.
	- Redshift stores these snapshots internally in Amazon S3
	- Supports Cross-Region snapshots. 
- Regulatory compliance.


# Amazon Redshift Spectrum:
- Enables you to query and retrieve structured and semistructured data from files in Amazon S3 without having to load the data into Amazon Redshift tables.
- Redshift Spectrum queries employ massive parallelism to execute very fast against large datasets. Much of the processing occurs in the Redshift Spectrum layer, and most of the data remains in Amazon S3.
- Multiple clusters can concurrently query the same dataset in Amazon S3 without the need to make copies of the data for each cluster. 
- Redshift Spectrum can potentially use thousands of instances to take advantage of massively parallel processing. 
- You create Redshift Spectrum tables by defining the structure for your files and registering them as tables in an external data catalog. The external data catalog can be AWS Glue, the data catalog that comes with Amazon Athena, or your own Apache Hive metastore. 
