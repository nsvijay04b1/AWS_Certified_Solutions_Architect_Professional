# AWS CodeDeploy:
- A deployment service that automates application deployments to Amazon EC2 instances, on-premises instances, serverless Lambda functions, or Amazon ECS services.
- You can deploy a nearly unlimited variety of application content, including: Code, Lambda functions, Web and configuration files, Executables, Packages, Scripts, Multimedia files, …etc.
- Can deploy from Amazon S3 buckets, GitHub repositories, or Bitbucket repositories.
- For an in-place deployment, CodeDeploy performs a rolling update across Amazon EC2 instances. 
- You can automatically or manually stop and roll back deployments if there are errors. 

Application Deployment strategies:
- In-place deployment: The application on each instance in the deployment group is stopped, the latest application revision is installed, and the new version of the application is started and validated. 
- Blue/green (or A/B) deployment: 
	- Two identical production environments work in parallel.
	- One environment  is the currently-running production environment receiving all user traffic (Blue environment).
	- The other environment is a clone of it, but idle (Green).
	- Both use the same database back-end and app configuration:
	- The new version of the application is deployed in the green environment and tested for functionality and performance.
	- Once the testing results are successful, application traffic is routed from blue to green. Green then becomes the new production.
- Canary deployment:
	- you deploy a new application code in a small part of the production infrastructure.
	- Only a few users are routed to it. This is why it is called Canary deployment as it refers to the canary birds used by miners to detect toxic gaz. This is actually testing on real users.
	- Users are usually selected carefully: low impact users or users that are ready to share feedback. 
	- If no errors reported, the new version can gradually roll out to the rest of the infrastructure.
- Rolling deployment:
	- an application’s new version gradually replaces the old one.
	- The actual deployment happens over a period of time.
	- During that time, new and old versions will coexist without affecting functionality or user experience. 
	- Unlike the Canary deployment strategy, the phasing is based on parts of the infrastructure and not on groups of users.