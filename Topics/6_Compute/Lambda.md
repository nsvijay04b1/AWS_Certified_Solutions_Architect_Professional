# Lambda:
- a serverless compute service that runs your code in response to events and automatically manages the underlying compute resources for you.
- Functions are “stateless,” with no affinity to the underlying infrastructure.
- Functions are triggered by events, by HTTP requests or on a schedule.
- When Lambda runs your function:
	- it invokes a handler function in you code that you defined when creating the Lambda function, and passes an event object and a context object to this handler.
	- Event: Lambda passes the event data to your function in JSON format. 
	- Context: This object provides methods and properties that provide information about the invocation, function, and execution environment. 
- Sizing:
	- You define the amount of memory available to your Lambda function at runtime. 
	- Lambda allocates CPU power and network capacity in proportion to the amount of memory configured. 
	- Lambda is charged in increments of 100ms.
- Multi-AZ.
- You can set the timeout up to 15mn (max allowed time for a function to end).

## Lambda function code:
- Natively supports Java, Go, PowerShell, Node.js, C#, Python, and Ruby code, and provides a Runtime API which allows you to use any additional programming languages to author your functions.
- Lambda Supports also packaging and deploying functions as container images.
- Supports Code signing. To verify code integrity, use AWS Signer to create digitally signed code packages for functions.
- Function code package is stored in S3.
- Function code package has a size limit. If you need more size, you can use Lambda Layers.
- Layers:
	- A layer is a .zip file archive that contains libraries, a custom runtime, or other dependencies.
	- With layers, you can use libraries in your function without needing to include them in your deployment package. 
	- Layers not supported for containers.
	- A function can use up to five layers at a time. 
	- Layers are extracted to the /opt directory in the function execution environment. 

Lambda has three events integration methods:
- Lamba reads events: For services that generate a queue or data stream. Lambda reads data from the other service, creates an event, and invokes your function. Supported services: DynamoDB streams , SQS, MQ, Kinesis, Apache Kafka (Amazon-managed or self-managed).
- Services that invoke Lambda functions synchronously: the other service waits for the response from your function. Supported services: ELBv2, Cognito, Lex, Alexa, API Gateway, CloudFront, Kinesis Data Firehose, S3 Batch.
- Services that invoke Lambda functions asynchronously: Lambda queues the event before passing it to your function. The other service gets a success response as soon as the event is queued. Supported Services: S3, SNS, SES, CloudFormation, CloudWatch, CodeCommit, IoT, CodePipeline.

## Lambda Scaling:
- The first time you invoke your function, AWS Lambda creates an instance of the function and runs its handler method to process the event. 
- When the function returns a response, it stays active and waits to process additional events. 
- If you invoke the function again while the first event is being processed, Lambda initializes another instance, and the function processes the two events concurrently. 
- As more events come in, Lambda routes them to available instances and creates new instances as needed. 
- When the number of requests decreases, Lambda stops unused instances to free up scaling capacity for other functions.

## Lambda Concurrency:
- Concurrency is the number of requests that your function is serving at any given time.
- Concurrency is subject to a Regional quota that is shared by all functions in a Region. 
- Reserved Concurrency:
	- When a function has reserved concurrency, no other function can use that concurrency. 
	- Also limits the maximum concurrency for the function, and applies to the function as a whole, including versions and aliases. 
- Provisioned concurrency:
	- Initializes a requested number of execution environments so that they are prepared to respond to your function's invocations.
	- Ensures that all requests are served by initialized instances with very low latency. 
	- Provisioned concurrency counts towards a function's reserved concurrency and Regional quotas. 
	- When all provisioned concurrency is in use, the function scales up normally to handle any additional requests. 
- When requests come in faster than your function can scale, or when your function is at maximum concurrency, additional requests fail with a throttling error (HTTP 429 Too Many Requests). 

## Lambda Application Auto Scaling:
- Application Auto Scaling takes a step further by providing autoscaling for provisioned concurrency. 
- Lets you create a target tracking scaling policy that adjusts provisioned concurrency levels automatically, based on the utilization metric that Lambda emits.

## Lambda RDS Proxy:
- You can use RDS Proxy: manages a pool of database connections and relays queries from a function. 
- This enables a function to reach high concurrency levels without exhausting database connections. 
- Supported on MySQL and Aurora.

## File System for Lambda:
- You can configure a function to mount an EFS file system to a local directory.
- A function connects to a file system over the local network in a VPC.
- The function subnet must be in the same AZ as the EFS mount point.

## Lambda Network:
- You can configure a Lambda function to connect to private subnets in a VPC.
- When you connect a function to a VPC, Lambda creates an elastic network interface for each subnet in your function's VPC configuration.
- Multiple functions connected to the same subnets share network interfaces. 
- Connecting additional functions to a subnet that has an existing Lambda-managed network interface is much quicker than having Lambda create additional network interfaces. 
- However, if you have many functions or functions with high network usage, Lambda might still create additional network interfaces. 
- If you configure a Lambda function in your VPC and you need your function to have access to internet, you need to do the necessary in your VPC (NAT gateway, Security Group, …).

## Lambda interface VPC endpoints:
- Can be created to invoke your Lambda function from you VPC without crossing the public internet. 
- Each interface endpoint is represented by one or more elastic network interfaces in your subnets. 
- A network interface provides a private IP address that serves as an entry point for traffic to Lambda. 

## Lambda Logging:
- Lambda automatically monitors Lambda functions on your behalf and sends function metrics to Amazon CloudWatch.
- Lambda sets a CloudWatch Logs log group and a log stream for each instance of your function. Permissions required: CreateLogGroup, CreateLogStream, and PutLogEvents.
- All output (stdout & stderr) from you function is sent to the log stream.

## Lambda@Edge:
- is a feature of Amazon CloudFront that lets you run code closer to users of your application, which improves performance and reduces latency. 
- runs your code in response to events generated by the Amazon CloudFront content delivery network (CDN). 
- Refer to the CloudFront notes for more details.