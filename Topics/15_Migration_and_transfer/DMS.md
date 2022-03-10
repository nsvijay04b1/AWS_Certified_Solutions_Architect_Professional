# Database Migration Options:


## RDS Migration (MySQL & MariaDB):
- If your scenario supports it, it is easier to move data in and out of Amazon RDS by using backup files and Amazon S3. 
- Use cases:
	- Migrate data from an on-prem MySQL or MariaDB live database to AWS RDS. You need therefore to minimize the impact on application availability.
	- Migrate a very large on-prem MySQL or MariaDB database to RDS. You need to reduce the cost of the import by reducing the amount of data that is passed across the network to AWS. 
-Procedure:
	- Use the mysqldump utility to create a database backup in either SQL or delimited-text format.
	- Compress your database backup file,
	- Transfer your compressed database backup to an Amazon EC2 instance. You can use scp or an SSH client to copy the file. 
	- Import the data into a new Amazon RDS DB instance (same region as the EC2 instance).
	- You then use replication to bring the Amazon RDS DB instance up-to-date with your live on-prem instance, before redirecting your application to the Amazon RDS DB instance. 

## RDS SQL Server Migration:
- If your database can be offline while the backup file is created, copied, and restored, we recommend that you use native backup and restore to migrate it to RDS:
	- You can create a full backup from your local server using full backup files (.bak files).
	- Transfer the backup file to S3.
	- Restore the backup from S3 onto an existing Amazon RDS DB instance. 
- If your on-premises database can't be offline, we recommend that you use the AWS Database Migration Service to migrate your database to Amazon RDS.

RDS Oracle Migration:
- How you import Oracle data into an Amazon RDS DB instance depends on the amount of data you have and the number and variety of database objects in your database:
	- You can use Oracle SQL Developer to import a simple, 20 MB database. 
	- You can use Oracle Data Pump to import complex databases, or databases that are several hundred megabytes or several terabytes in size.
	- You can use AWS Database Migration Service (AWS DMS) to migrate databases without downtime and continue ongoing replication until you are ready to switch over to the target database.


# AWS Database Migration Service (AWS DMS):
- A cloud service that makes it easy to migrate relational databases, data warehouses, NoSQL databases, and other types of data stores.
- You can use AWS DMS to migrate your data into the AWS Cloud, between on-premises instances (through an AWS Cloud setup), or between combinations of cloud and on-premises setups. 
- With AWS DMS, you can perform one-time migrations, and you can replicate ongoing changes to keep sources and targets in sync.
- Uses an AWS DMS replication instance: a managed EC2 instance that runs replication tasks.
- If you want to change database engines, you can use the AWS Schema Conversion Tool (AWS SCT) to translate your database schema to the new platform. You then use AWS DMS to migrate the data. 
- AWS DMS supports nearly all of today's most popular DBMS engines as data sources, including Oracle, Microsoft SQL Server, MySQL, MariaDB, PostgreSQL, Db2 LUW, SAP, MongoDB, and Amazon Aurora. 
- AWS DMS provides a broad coverage of available target engines including Oracle, Microsoft SQL Server, PostgreSQL, MySQL, Amazon Redshift, SAP ASE, Amazon S3, and Amazon DynamoDB. 
- Change Data Capture (CDC):
	- You can create an AWS DMS task that captures ongoing changes to the source data store while you are migrating your data.
	- You can also create a task that captures ongoing changes after you complete your initial (full-load) migration to a supported target data store. 
	- This process is called ongoing replication or change data capture (CDC). AWS DMS uses this process when replicating ongoing changes from a source data store. 
	- This process works by collecting changes to the database logs using the database engine's native API. 
- You can use DMS and AWS Snowball Edge to migrate large Databases from on-prem to AWS:
	- You use the AWS SCT and DMS to extract the data locally and move it to a Snowball Edge device.
	- You ship the Edge device or devices back to AWS. The Edge device automatically loads its data into an Amazon S3 bucket.
	- AWS DMS takes the files from S3 and migrates the data to the target data store.
	- If you are using CDC, those updates are written to the Amazon S3 bucket and then applied to the target data store.


## SCT - Schema Conversion Tool

- SCT is a standalone app used for converting one database engine to another including conversion of schema from a DB to S3
- SCT is not used when migrating between DBs of the same type
- SCT works with OLTP DBS (MySQL, Oracle, Aurora, etc.) and OLAP databases (Teradata, Oracle, Vertica, Greenplum, etc.)

## DMS and Snowball

- Larger migrations might imply moving dbs with sizes of multi-TB
- Moving data over networks takes time and consumes capacity
- DMS is able to utilise Snowball products to migrate databases
- Migration steps:
    1. Use SCT to extract data locally and move the data to a Snowball
    2. Ship the device back to AWS. They will load the data into an S3 bucket
    3. DMS migrates from S3 into a target source
    4. Change Data Capture (CDC) can capture changes and via S3 intermediary they are also written to the target database