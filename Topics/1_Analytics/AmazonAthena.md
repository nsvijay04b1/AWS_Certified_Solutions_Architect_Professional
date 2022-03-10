# Athena:
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


Athena vs Redshift Spectrum:
- The basic functionality offered by Athena and Redshift Spectrum is similar: querying S3 using standard SQL, and storing the results of that query.
- The main differences are:
	- Resource provisioning: While both are serverless, Athena relies on pooled resources provided by AWS, whereas Spectrum resources are allocated according to your Redshift cluster size. You have therefore more control over performance with Redshift Spectrum.
	- Loading data into Redshift: Athena stores query results on S3, and they can be loaded into Redshift from there; whereas Spectrum can be used to join tables stored on Redshift directly.
