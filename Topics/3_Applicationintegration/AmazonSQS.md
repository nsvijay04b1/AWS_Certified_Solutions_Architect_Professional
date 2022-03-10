Amazon Simple Queue Service (SQS):
- offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components. 
- Push-pull mechanism.
- You control who can send messages to and receive messages from an Amazon SQS queue.
- Two Queue types:
	- Standard: Best effort ordering, at-least-once message delivery, unlimited throughput.
	- FIFO: FIFO ordering, exactly-once message processing, limited throughput: 300 API calls per second, up to 10 messages per API call. So max is 3,000 messages per second.
- Messages:
	- Each message gets a Message ID.
	- You can use message attributes to attach custom metadata to messages. 
	- Max size: 256 KB.
	- A message can be a pointer to an S3 or DynamoDB object.
	- Messages are stored on multiple servers.
	- Retention period: 1mn to 14 days. Default is 4 days.
	- Messages can be protected by Server-Side Encryption using KMS.
- Concurrency:
	- Multiple producers and consumers can use the same queue at the same time. 
	- When a message is received/collected by a consumer, it remains in the queue.
	- To prevent other consumers from processing the message again, Amazon SQS sets a visibility timeout, a period of time during which Amazon SQS prevents other consumers from receiving and processing the message. Can be 0 sec to 12 hours. Default is 30 seconds. The consumer should delete the message from the queue after processing it.
- Short Polling & Long Polling:
	- SQS provides short polling (no wait) and long polling (with wait) to receive messages from a queue.
	- By default, queues use short polling.
- Short Polling:
	- The ReceiveMessage request queries only a subset of the servers (based on a weighted random distribution) to find messages that are available to include in the response. 
	- Amazon SQS sends the response right away, even if the query found no messages.
	- To set Short polling: either in the queue parameter (WaitTimeSeconds = 0) or the request parameter (ReceiveMessageWaitTimeSeconds = 0).
- Long Polling:
	- The ReceiveMessage request queries all of the servers for messages. 
	- SQS sends a response after it collects at least one available message, up to the maximum number of messages specified in the request. 
	- SQS sends an empty response only if the polling wait time expires. 
	- The maximum long polling wait time is 20 seconds.
	- Long polling helps reduce the cost of using Amazon SQS by eliminating the number of empty responses.
- Delay queues let you postpone the delivery of new messages to a queue for a number of seconds.
- Message timers let you specify an initial invisibility period for a message added to a queue.
- A dead-letter queue is a queue that one or more source queues can use for messages that are not consumed successfully. 
- Supports SSE using KMS CMKs.
- Supports anonymous access.
- Lambda Triggers:
	- Lambda polls the queue and invokes your Lambda function synchronously with an event that contains queue messages.
	- Lambda reads messages in batches and invokes your function once for each batch: the event that is passed to your function contains all read messages.
	- You can set the batch size: number of messages that Lambda will collect at once.
	- You can set the batch window: period that Lambda should wait to collect the messages before invoking your function. Max = 5mn.
	- Lambda continues to poll messages from the SQS standard queue until batch window expires, the payload limit is reached or full batch size is reached. 
	- When your function successfully processes a batch, Lambda deletes its messages from the queue. 


## SQS Standard vs FIFO Queues

- Standard: 
    - Messages are delivered at least once
    - The order of the delivery is not guaranteed (best-effort ordering)
    - They don't have performance limitations
- FIFO: 
    - Messages are delivered exactly once
    - The order of the delivery is guaranteed to be the same as the messages were sent
    - Performance is limited: 3000 messages per second with batching or up to 300 messages per second without batching
    - There is also available a high throughput mode for FIFO queues
    - FIFO queues have to have `.fifo` suffix in order to be valid FIFO queues

## SQS Extended Client Library

- SQS has a message size limit of 256KB
- Extended Client Library can be used when we want to send messages larger than this size
- It can process large payloads and have the bulk of the payloads stored in S3
- When we send a message using `SendMessage` API, the library uploads the content to S3 and stores a link to this content in the message
- When receiving a message, the library loads the payload from S3 and replaces the link with the payload in the message
- When deleting a message from a queue, the large S3 payload will also be deleted
- The Extended Client Library has an implementation in Java

## SQS Delay Queues

- Delay queues allow us to postpone the delivery of messages in SQS queues
- For a delay queue we configure a `DelaySeconds` value. Messages added to the queue will be invisible for the amount of `DelaySeconds`
- The default value of `DelaySeconds` is 0, the max value is 15 minutes. In order for a queue to be delay queue, the value should be set to be greater than 0
- Message times allow a per-message basis invisibility to be set, overriding the queue setting. Min/max values are the same. Per message setting is not supported for FIFO queues


## SQS Dead Letter Queues (DLQ)

- Dead Letter Queues are designed to help handle reoccurring failures while processing messages from a queue
- Every time a message is received, the `ReceiveCount` is incremented. In order to not process the same message over and over again, we define a `maxReceiveCount` and a DLQ for a queue. After the number of retries is greater than the `maxReceiveCount`, the message is pushed into the DLQ
- Having a DLQ, we can define alarms to get notified in case of a failure
- Messages in DLQ can be further analyzed
- Every queue (including DLQ) have a retention period for messages. Every message has an `mq-timestamp` which represents when was the message was added to the queue. In case a message is moved into a DLQ, this timestamp is not altered
- Generally the retention period for a DLQ should be longer compared to normal queues
- A single DLQ can be used for multiple sources