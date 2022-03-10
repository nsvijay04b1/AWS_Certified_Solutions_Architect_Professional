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


Q: When should I use AWS Step Functions vs. Amazon Simple Workflow Service (SWF)?
- You should consider using AWS Step Functions for all your new applications, since it provides a more productive and agile approach to coordinating application components using visual workflows.
- If you require external signals to intervene in your processes, or you would like to launch child processes that return a result to a parent, then you should consider Amazon SWF.
- With Amazon SWF, instead of writing state machines in declarative JSON, you write a decider program to separate activity steps from decision steps. This provides you complete control over your orchestration logic, but increases the complexity of developing applications. 

