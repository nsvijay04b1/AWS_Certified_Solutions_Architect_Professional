# AWS Step Functions:
- A serverless orchestration service that lets you combine AWS Lambda functions and other AWS services to build business-critical applications.
- Through Step Functions' graphical console, you see your application’s workflow as a series of event-driven steps. 
- Step Functions is based on state machines and tasks.
- A state machine is a workflow.
- Each step in a workflow is a state. 
- The workflow is written using the Amazon States Language (JSON formatted).
- Two types of state machines/workflows: 
	- Standard workflow: can run for up to 1 year, up to 2,000 executions per second, employs an at-most-once model.
	- Express workflow: can run for up to 5 minutes, up to 100,000 executions per second, employs an at-least-once model (can have duplicate executions). A popular use case is ingesting high throughput IoT messages.
- A task is a state in a workflow that represents a single unit of work that another AWS service performs.
- Callback tasks:
	- Callback tasks provide a way to pause a workflow until a task token is returned. 
	- A task might need to wait for a human approval, integrate with a third party, or call legacy systems. 

## States

- Available states:
    - `SUCCEED` and `FAIL`
    - `WAIT`: wait for a certain period or time or until a date
    - `CHOICE`: allows a state machine to take a different path depending on an input
    - `PARALLEL`: allows to create parallel branches in a state machine
    - `MAP`: expects a list of things, for which it might perform a certain set of things
    - `TASK`: represents a single unit of work performed by a state machine. They can be integrated with: Lambda, Batch, DynamoDB, ECS, SNS, SQS, Glue, SageMaker, EMR, etc.


