# ECS:
- A fully managed container orchestration service for Docker. 
- Allows you to easily run applications on a managed cluster either on EC2 instances or AWS Fargate. 
- ECS on EC2 instances:
	- You create an ECS cluster: group of EC2 instances that you manage.
	- ECS will install Docker and a lightweight agent on each EC2 instance.
	- You can choose the AMI you want.
	- When using ECS on EC2 instances, you have root access to the operating system of your container instances, enabling you to take ownership of the operating system’s security settings as well as load and configure additional software components for security capabilities such as monitoring, patch management, log management and host intrusion detection.
	- Use case: Customers who require greater control of their EC2 instances to support compliance and governance requirements or broader customization options.
	- Containers are located in container registries (ECR, DockerHub)
- ECS uses **container definitions** to locate images in the container registries, which port should the image use, etc. providing information about the container we want to run
- **Task definition**: represents a self-contained application, can have one or many containers defined in it. A task in ECS defines the application as a whole
- Task definitions store the resources to be used (CPU, memory), networking configuration, compatibility (EC2 mode or Fargate) and also they store the task role (IAM role)
- Task role give permission to ECS containers to access AWS services
- A task does not scale by its own and it is not HA
- ECS service: it is configured by a **service definition**. In a service we define how we want a task to scale, how many copies we like to run
- ECS services define scalability and HA for tasks
- **Tasks**:
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

## ECS Cluster Modes

- Cluster mode define how much of the admin overhead is required for running containers in ECS (what parts do we manage and what parts does AWS manage)
- Cluster modes are:
    - EC2 Mode
    - Fargate Mode

### EC2 Mode

- Uses EC2 instances which are running inside of a VPC
- Since we are inside of a VPC, we can benefit from using multiple AZs
- When we create the cluster, we specify the initial size of containers
- Horizontal scaling for EC2 instances and for ECS tasks is controlled by ASGs
- With EC2 cluster mode we are paying for EC2 instances independently of what containers and how many of containers are running on them

### Fargate Mode

- We don't have to manage EC2 instances for use as container host
- With Fargate there are no servers to manage
- AWS maintains a shared Farge infrastructure platform offered to all users
- We gain access to resources from a shared pool, we don't have visibility for other customers
- A Fargate deployment still uses a cluster with a VPC which operates in AZs
- ECS tasks are injected into the VPC with an ENI, they are running on the Fargate shared platform
- With Fargate mode we only pay for the containers we using based on the resources they consume

## EC2 vs ECS (EC2) vs Fargate

- If we are already using containers, we should use ECS
- Containers make sense if we want to isolate applications
- We generally pick EC2 mode if we have a large workload and the business is price conscious
- Historically EC2 mode was giving the most value for the price if we were using saving plans. Nowadays we can have savings plan for Fargate and Lambda, so we should default ot Fargate instead of EC2 mode
- If we are overhead conscious, we should use Fargate
- For small/burst workloads we should use Fargate as well. Same is recommended for batch/periodic workloads