# AWS Lambda
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

- Lambda is a Function-as-a-Service (FaaS) product. We provide specialized short running focused code for Lambda and it will take care running it and billing us for only what we consume
- Every Lambda function uses a supported runtime, example: Python 3.8, Java 8, NodeJS
- Every Lambda function is loaded into an executed in a runtime environment
- When we create a function we define the resources the function will use. We define the memory directly and CPU usage allocation indirectly (based on the amount of memory)
- We are only billed for the duration the function is running based on the number of invocations and the resources specified
- Lambda is a key part of serverless architectures in AWS
- Lambda has support for the following runtimes:
    - Python
    - Ruby
    - Go
    - Java
    - C#
    - Custom using Lambda layers
- Lambda functions are stateless, meaning there is no data left over after an invocation
- When creating a Lambda function we define the memory. The memory can be between 128MB and 3GB in 64MB steps(historically, nowadays there is support for up to 10GB)
- We do not directly define the vCPU allocated to each function, it will automatically scale with the memory: 1792MB of memory gives 1 vCPU
- The runtime env. has a 512MB storage available as `/tmp`
- Lambda function can run up to 15 minutes, after this a timeout will occur

## Lambda Networking

- Lambda functions can have 2 types of networking modes:
    - Public (default):
        - Lambda can access public AWS services such as SQS, DynamoDB, etc. and also internet based services
        - Lambda has network connectivity to public services running on the internet
        - Offers the best performance for Lambda, no customer specific networking is required
        - With public networking mode Lambda function wont be able to access resources in a VPC unless the resources do have public IPs and security controls allow external access
    - VPC Networking:
        - Lambda functions will run inside a VPC, so they will access everything in a VPC, assuming NACLs and SGs allow access
        - They wont be able to access services outside of the VPC, unless networking configuration exists in the VPC to allow external access
        - At the creation of the function, a certain ENI might be created for accessing the VPC. The initial setup would take up to 90 seconds

## Lambda Security

- There are 2 key parts of the security model
    - Lambda Functions will assume an execution role in order to access other AWS resources
    - Resources policies: similar to resource policies for S3. Allows external accounts to invoke a Lambda functions, or certain services to use Lambda functions. Resources polices can be modified using the CLI (not with the console)

## Lambda Logging

- Lambda uses CloudWatch Logs and X-Ray
- Logs from Lambda executions are stored in CloudWatch Logs
- Details about Lambda metrics are stored in CloudWatch Metrics
- Lambda can be integrated with X-Ray for distributed tracing
- For Lambda to be able to log we need to give an execution log to it

## Lambda Invocations

- There are 3 ways Lambda functions can be invoked:
    - **Synchronous invocation**:
        - Command line or API directly invoking the function
        - The CLI or API will wait until the function returns
        - API Gateway will also invoke Lambdas synchronously, use case for many serverless applications
        - Any errors or retries have to be handled on the client side
    - **Asynchronous invocation**:
        - Used typically when AWS services invoke the function (example: S3 events)
        - The service will not wait for the response (fire and forget)
        - Lambda is responsible for any failure. Reprocessing will happen between 0 and 2 times
        - The function should be idempotent in order to be rerun
        - Lambda can be configured to send events to a DLQ in case of the processing did not succeed after the number of retries
        - Destination: events processed by Lambdas can be delivered to destinations like SQS, SNS, other Lambda, EventBride. Success and failure events can be sent to different destinations
    - **Event Source mapping**:
        - Typically used on streams or queues which don't generate events (Kinesis, DynamoDB streams, SQS)
        - Event Source mappers polls these streams and retrieves batches. These batches can be broken in pieces and sent to multiple Lambda invocations for processing
        - We can not have a partially successful batch, either everything works or nothing works
- In case of event processing in async invocation, in order to process the event we don't explicitly need rights to read from the sender
- In case of event source mapping the event source mapper is reading from the source. The event source mapping uses permissions from the Lambda execution role to access the source service
- Even if the function does not read data directly from the stream, the execution role needs read rights in order to handle the event batch
- Any batch that consistently fails to be processed, it can be sent to an SQS queue or SNS topic for further processing

## Lambda Versions

- We can define different versions for given functions
- A version of a function is the code + configuration of the function
- When we publish a version, it becomes immutable, it no longer can be changed. It event gets its own ARN (Amazon Resource Name)
- `$Latest` points to the latest version of Lambda version (it is not immutable)
- We can also define aliases (DEV, STAGE, PROD) which point to a version of the function. Aliases can be changed to point to other versions

## Lambda Start-up Times

- Lambda code runs inside of a runtime environment (execution context)
- At first invocation this execution context needs to be created and this will take time
- This process is known as cold start and it can take 100ms or more
- If the function is invoked again without too much of a gap, it might use the same execution context. This is called warm start
- One function invocation runs in an execution environment at a time. If multiple parallel instances are needed, the contexts will require cold starts
- Provisioned concurrency: we provision execution context in advance for Lambda invocations
- The improve performance we can use the `/tmp` folder to download data to it. If another invocations uses the same execution context, it will be able to access the previously downloaded data
- We can create database connections outside of the Lambda handler. These will also be available for other invocations afterwards

## Lambda Handler

- Lambda function executions have lifecycles
- The function code runs inside an execution environment
- Lifecycle phases:
    - INIT: creates on unfreezes the execution environment
    - INVOKE: runs the function handler (cold start)
    - NEXT INVOKE(s): warm start using the same environment
    - SHUTDOWN: execution environment is terminated

## Lambda Versions

- Unpublished functions can be changed and deployed
- `$LATEST` version of the Lambda code can be edited and deployed
- We can take the current state of the function and publish it which will create an immutable version
- If the function is published, the code, dependencies, runtime settings and env. variables in the version created can not be edited
- Each version gets an uniq ARN (Qualified ARN)
- Unqualified ARN points at the function without a specific version (`$LATEST`)

## Lambda Aliases

- An alias is a pointer to a function version
- Example: PROD => function:1, BETA => function:2
- Each alias has an unique ARN
- Aliases can be updated, changing which version they reference
- Useful for PROD/DEV, BLUE/GREEN deployments, A/B testing
- We can also use alias routing: sending a certain percentage of request to v1 and other percentage to v2. Both versions need the same role, same DLQ will be used and both versions need to be published

## Lambda Environment Variables

- Key and value pairs for associated with Lambda functions
- By default they are associated with `$LATEST` - can be edited
- If they are publishes, they can not be edited
- They can be accessed within the execution environment
- The environment variables can be encrypted with KMS
- They allow code execution to be adjusted based on variables

## Lambda Layers

- Used to split off libraries and dependencies from Lambda functions
- Reduces the size of the deployment package
- Layers can be reused by multiple Lambda functions
- Libraries in layers are extracted in the `/opt` folder
- Layers allow new runtimes which are not explicitly supported by AWS

## Lambda Container Images

- Until recently Lambda was considered to be a Function as a Service (FaaS) product, which means creation of a function, uploading code and executing it
- Many organizations use containers and CI/CD processes built for containers
- Lambda is now capable to use containers images
- It is an alternative way of packaging the function code and using it with the Lambda product
- Lambda Runtime API - has to be included with the container images
- AWS Lambda Runtime Interface Emulator (RIE): used for local Lambda testing

## Lambda and ALB

- Multi-Value headers: 
    - Example: `http://function.io?&search=roffle&search=winkie
    - Without multi-value headers the Lambda receives the following:
        ```
        "queryStringParameters": {
            "search": "winkie"
        }
        ```
    - If the multi-value headers are enabled, we get this delivered to Lambda:
        ```
        "multiValueQueryStringParameters": {
            "search": ["roffle", "winkie"]
        }
        ```

## AWS SAM - Serverless Application Model

- Serverless application is not just Lambda function, it can include many more services such as:
    - Front end code and assets hosted by S3 and CloudFront
    - API endpoint - API Gateway
    - Compute - Lambda
    - Database - DynamoDB
    - Event sources, permissions and more...
- SAM group of products has 2 main parts:
    - AWS SAM template specification, which is an extension of CloudFormation
    - AWS SAM CLI allowing local testing, local invocation and deployments into AWS

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