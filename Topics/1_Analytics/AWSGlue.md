# AWS Glue:
- A serverless data integration service that makes it easy to discover, prepare, and combine data for analytics, machine learning, and application development. 
- Provides both visual and code-based interfaces to make data integration easier. 
- ETL (extract, transform, and load) developers can visually create, run, and monitor ETL workflows with a few clicks in AWS Glue Studio. 
- AWS Glue generates the code that's required to transform your data from source to target. 
- Data Sources: S3, RDS, DynamoDB, any JDBC DB, MongoDB.
- Data Targets: S3, RDS, any JDBC DB, MongoDB.
- Supports data streams as data sources such as Kinesis Data Streams and Apache Kafka
- Data targets supported: S3, RDS, JDBC data sources

## Data Catalog

- Collection of persistent metadata about data sources in a region
- Provides one uniq data catalog in every region per account
- It helps avoid data silos: makes data not visible in an organization be visible a able to be browsed
- Data Catalog can be used by Amazon Athena, Redshift Spectrum, EMR and AWS Lake Formation
- Data is discovered by configuring crawlers and givin it credentials to access data sources

## Glue Job

- Extract, Transform an Load jobs
- Jobs can do transformation by using scripts created by us
- Jobs are serverless, AWS maintains a pool of resources
- We are only billed by resources we consume