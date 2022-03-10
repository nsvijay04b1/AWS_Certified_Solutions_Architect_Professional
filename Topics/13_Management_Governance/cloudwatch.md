# CloudWatch:
- (Should be called "CloudWatch Metrics").
- Monitors AWS resources and your applications in real time to collect and track metrics. 
- Generates statistics and graphs.
- If you put your own custom metrics into the repository, you can retrieve statistics on these metrics as well. 
- The CloudWatch home page automatically displays metrics about every AWS service you use. You can additionally create custom dashboards. 
- CloudWatch concepts: Namespaces, Metrics, Dimensions, Statistics, Percentiles, Alarms.
- Namespace: 
	- a container for CloudWatch metrics. Example: "AWS/EC2".
	- Metrics in different namespaces are isolated from each other, so that metrics from different applications are not mistakenly aggregated into the same statistics. 
- Metric: 
	- a time-ordered set of data points that are published to CloudWatch. 
	- Think of a metric as a variable to monitor, and the data points as representing the values of that variable over time. 
	- Metrics exist only in the Region in which they are created. 
	- Metrics cannot be deleted, but they automatically expire after 15 months.
	- Each data point in a metric has a time stamp, and (optionally) a unit of measure.
	- Metrics resolution:
		- Standard resolution: data having a one-minute granularity. All metrics produced by AWS services are standard resolution by default. 
		- High resolution: data at a granularity of one second.
- Dimension:
	- A dimension is a name/value pair that is part of the identity of a metric.
	- You can assign up to 10 dimensions to a metric. 
	- For example, you can get statistics for a specific EC2 instance by specifying the InstanceId dimension when you search for metrics. 
- Statistics:
	- Statistics are metric data aggregations over specified periods of time. 
	- Aggregations are made using the namespace, metric name, dimensions, and the data point unit of measure, within the time period you specify. 
	- Example of statistics functions: min, max, average, sum, percentile.
- Percentile:
	- A percentile indicates the relative standing of a value in a dataset.
	- For example, the 95th percentile means that 95 percent of the data is lower than this value and 5 percent of the data is higher than this value. 
	- Some CloudWatch metrics support percentiles as a statistic. 
- Alarm:
	- Automatically initiate actions on your behalf. 
	- An alarm watches a single metric over a specified time period, and performs one or more specified actions, based on the value of the metric relative to a threshold over time. 
	- You can also add alarms to dashboards. 
- Action:
	- An action can be triggered either when:
		- the alarm goes to an "In-Alarm" state, or
		- the alarm goes to an "OK" state, or,
		- the alarm goes to an "Insufficient data" state.
	- Action types: SNS topic, Auto Scaling action, EC2 action or create OpsItems in Systems Manager Ops Center.
- Metrics retention for data points with period of:
	- less than 60 sec : 3 hours.
	- 1 mn : 15 days.
	- 5 mn : 2 months.
	- 1 hr : 15 months.
- You can use CloudWatch cross-Region functionality to aggregate statistics from different Regions. 
- CloudWatch agent: enables you to collect internal system-level metrics from Amazon EC2 instances and on-prem servers across operating systems. 



# CloudWatch Logs:
- Collects and monitors log files from sources like EC2, CloudTrail and Route53.
- Logs can be viewed, searched, filtered and archived.
- By default logs are stored permanently.
- A log event is a record of some activity. Composed of a timestamp and a raw message.
- A log stream is a sequence of log events that share the same source. 
- Log groups define groups of log streams that share the same retention, monitoring, and access control settings.
- Metrics from logs:
	- You can search and filter the log data coming into CloudWatch Logs and create one or more metric filters.
	- Metric filters define the terms and patterns to look for in log data as it is sent to CloudWatch Logs. 
	- CloudWatch Logs uses these metric filters to turn log data into numerical CloudWatch metrics that you can graph or set an alarm on.
- Subscriptions:
	- You can use subscriptions to stream log events from CloudWatch Logs to other services such as Kinesis stream, Kinesis Data Firehose stream, or AWS Lambda for custom processing, analysis, or loading to other systems. 
	- A subscription filter defines the filter pattern to use for filtering which log events get delivered to your AWS resource.
	- Each log group can have up to two subscription filters associated with it.
- Can be used across multiple AWS accounts (requires cross-account access).
- CloudTrail logs can be sent to CloudWatch Logs for monitoring. Uses the CloudWatch Logs APIs to create and send logs to a CloudWatch Log log stream. 

CloudWatch Logs Insights:
- Enables you to search and analyze your log data in CloudWatch Logs.
- Includes a purpose-built query language.
- Automatically discovers fields in logs from many AWS services and from 3rd party JSON logs.



# CloudWatch Events:
- Delivers a near real-time stream of system events that describe changes in AWS resources.
- Based on Amazon EventBridge (See "Queues & Buses" notes).
- CloudWatch Event is based on one special event stream included in EventBridge for AWS system events.
- Events can be triggered in CloudWatch Events when:
	- A change happens in your AWS environment. For example, EC2 generates an event when the state of an EC2 instance changes from pending to running.
	- You make API calls. These events are published by CloudTrail. 
	- You can generate custom application-level events and publish them to CloudWatch Events. 
	- You set up scheduled events that are generated on a periodic basis.
- Rules:
	- A rule matches incoming events and routes them to targets for processing.
	- A single rule can route to multiple targets, all of which are processed in parallel.
	- A rule can customize the JSON sent to the target, by passing only certain parts or by overwriting it with a constant.
- Targets:
	- A target processes events. 
	- Targets can include EC2 instances, Lambda functions, Kinesis streams, ECS tasks, Step Functions state machines, SNS topics, SQS queues, and built-in targets.
	- A target receives events in JSON format. 
- Event Patterns:
	- Rules use event patterns to select events and route them to targets. 
	- A pattern either matches an event or it doesn't.
	- Event patterns are represented as JSON objects with a structure that is similar to that of events


# AWS X-Ray:
- A service that collects data about requests that your application serves.
- Provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization.
- For any traced request to your application, you can see detailed information not only about the request and response, but also about calls that your application makes to downstream AWS resources, microservices, databases and HTTP web APIs. 
