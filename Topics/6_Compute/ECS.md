# ECS:
- A fully managed container orchestration service for Docker. 
- Allows you to easily run applications on a managed cluster either on EC2 instances or AWS Fargate. 
- ECS on EC2 instances:
	- You create an ECS cluster: group of EC2 instances that you manage.
	- ECS will install Docker and a lightweight agent on each EC2 instance.
	- You can choose the AMI you want.
	- When using ECS on EC2 instances, you have root access to the operating system of your container instances, enabling you to take ownership of the operating system’s security settings as well as load and configure additional software components for security capabilities such as monitoring, patch management, log management and host intrusion detection.
	- Use case: Customers who require greater control of their EC2 instances to support compliance and governance requirements or broader customization options.
- Tasks:
	- A Task is an application. Tasks allow you to define a set of containers that you would like to be placed together.
	- A task definition is a JSON file that describes one or more containers (up to a maximum of ten) that form your application.
	- A task definition is like a blueprint for your application. Each time you launch a task in Amazon ECS, you specify a task definition. 
	- Task definitions include: the docker image, CPU and RAM amount, docker networking mode, data volumes, …
- ECS will place containers on the cluster based on your placement strategies.
- ECS CLI supports Docker Compose.
- Supports Amazon Elastic Container Registry (ECR), Docker Hub and 3rd party hosted Docker image repositories.
- Supports Blue/Green Deployments.
- Container auto-recovery.
- Uses EFS for storage persistence.
- Integration with ELB (ALB or NLB): automatically adds and removes containers from the load balancer. 

Sensitive data in ECS:
- Amazon ECS enables you to inject sensitive data (like credentials) into your containers by storing it in either "AWS Secrets Manager" secrets or "AWS Systems Manager Parameter Store" parameters and then referencing them in your container definition.
- Secrets can be exposed to a container in the following ways:
	- To inject sensitive data into your containers as environment variables, use the "secrets" container definition parameter.
	- To reference sensitive information in the log configuration of a container, use the "secretOptions" container definition parameter.
- Using AWS Secrets Manager: When you inject a secret as an environment variable, you can specify the full contents of a secret, a specific JSON key within a secret, or a specific version of a secret to inject. 
- Sensitive data is injected into your container when the container is initially started. If you change or rotate your secret, it will not reflect on the running container.

ECS Network Modes:
- awsvpc (recommended): The task is allocated its own elastic network interface (ENI) and a primary private IPv4 address. This gives the task the same networking properties as Amazon EC2 instances.
- bridge: The task utilizes Docker's built-in virtual network which runs inside each Amazon EC2 instance hosting the task.
- host: The task bypasses Docker's built-in virtual network and maps container ports directly to the ENI of the Amazon EC2 instance hosting the task. As a result, you can't run multiple instantiations of the same task on a single Amazon EC2 instance when port mappings are used.
- none: The task has no external network connectivity.

