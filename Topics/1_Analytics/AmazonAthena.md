# Amazon Athena

- It is a serverless interactive querying service
- We can take data stored in S3 and perform ad-hoc queries on the data paying only for the data consumed
- Athena uses a process named **Schema-on-read** - table-like translation
- Original data in S3 is never changed, it remains in its original form. It is translated to the predefined schema when it is read for processing
- Supported formats by Athena: XML, JSON, CSV/TSV, AVRO, PARQUET, ORC, Apache, CloudTrail, VPC Flowlogs, etc. Supports standard formats of structured data, semi-structured and unstructured data
- "Tables" are defined in advance in a data catalog and data is projected through when read. It allows SQL-like queries on data without transforming source data
- Athena has no infrastructure. We don't need set up anything in advance
- Athena is ideal for situations where loading/transforming data isn't desired
- It is preferred for querying AWS logs: VPC Flow Logs, CloudTrail, ELB logs, cost reports, etc
- Athena Federated Query: Athena now supports querying other data sources than S3. Requires a data source connector (AWS Lambda)

- an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL.
- Athena helps you analyze unstructured, semi-structured, and structured data stored in Amazon S3. Examples include CSV, JSON, or columnar data formats such as Apache Parquet and Apache ORC.
- Can be used to search logs in S3.
- How it works:
	- You specify the structure of your file and where the fields are. You can use the Data Catalog feature of AWS Glue to extract automatically this structure.
	- You submit a SQL request to Athena.
	- Athena instantiates a swarm of workers that read your files to extract the fields.
	- The SQL request is run against the collected fields.
- Serverless.
- Pay per query.
- Supports JDBC connections.
- Athena integrates with Amazon QuickSight for easy data visualization. 


## Athena vs Redshift Spectrum:
- The basic functionality offered by Athena and Redshift Spectrum is similar: querying S3 using standard SQL, and storing the results of that query.
- The main differences are:
	- Resource provisioning: While both are serverless, Athena relies on pooled resources provided by AWS, whereas Spectrum resources are allocated according to your Redshift cluster size. You have therefore more control over performance with Redshift Spectrum.
	- Loading data into Redshift: Athena stores query results on S3, and they can be loaded into Redshift from there; whereas Spectrum can be used to join tables stored on Redshift directly.
