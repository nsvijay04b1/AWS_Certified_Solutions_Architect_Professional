# Amazon Simple Workflow Service (SWF):
- Makes it easy to build applications that coordinate work across distributed components.
- A task represents a logical unit of work that is performed by a component of your application.
- Coordinating tasks across the application involves managing intertask dependencies, scheduling, and concurrency in accordance with the logical flow of the application. 
- You implement workers to perform tasks. 
- Workers can run either on cloud or on-prem.
- You can create tasks that are long-running, or that may fail, time out, or require restarts—or that may complete with varying throughput and latency. 
- Amazon SWF stores tasks and assigns them to workers when they are ready, tracks their progress, and maintains their state, including details on their completion.
- To coordinate tasks, you write a program that gets the latest state of each task from Amazon SWF and uses it to initiate subsequent tasks. 
- Amazon SWF maintains an application's execution state durably so that the application is resilient to failures in individual components.
- Unlike Step Functions, a user has to manage the infrastructure that runs the workflow logic and tasks. 
- Allows to build complex, automated and human involved workflows
- It is the predecessor of Step Functions, it uses instances/servers
- It allows the usage of the same patterns/anti patterns like long running workflows
- AWS recommends defaulting to Step Functions instead of SWF, use SWF only if there is very specific workflow that requires it
- Workflow: set of activities that carry out some objective together with the logic that coordinates the activities
- Within workflows we have activity task and activity workers
- A worker is program that we create to perform tasks
- Workflows have deciders, applications that run on an unit of compute of AWS
- Deciders schedule activity tasks, provides input data to activity workers, processes events and ends the workflow when the object is completed
- SWF workflows can run for max 1 year


## SWF vs Step Functions

- Default: Step Functions - they are serverless, they require lower admin overhead
- AWS FLow Framework - way of defining workflows supported by SWF
- External Signals to intervene in process, we need SWF
- Launch child flows and have the processing return to parent, we need ot use SWF
- Bespoke/complex decision logic: use SWF (custom decider application)
- Mechanical Turk integration: use SWF (suggested AWS architecture)
- You should consider using AWS Step Functions for all your new applications, since it provides a more productive and agile approach to coordinating application components using visual workflows.
- If you require external signals to intervene in your processes, or you would like to launch child processes that return a result to a parent, then you should consider Amazon SWF.
- With Amazon SWF, instead of writing state machines in declarative JSON, you write a decider program to separate activity steps from decision steps. This provides you complete control over your orchestration logic, but increases the complexity of developing applications. 

