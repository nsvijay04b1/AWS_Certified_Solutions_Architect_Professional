# EC2:

## EC2 Instance Types:
- General Purpose: M4 to M6 (fixed perf), T2 to T4 (burstable perf).
- Compute Optimized: C4 to C6.
- Memory Optimized: R4 to R6, X1, Z1.
- Storage Optimized: D2 to D3, i3.
- Accelerated: P2 to P4, G3 to G4.

## EC2 Nitro Systems:
- The Nitro System is a collection of AWS-built hardware and software components that enable high performance, high availability, and high security. 
- Based on Nitro hypervisor - A lightweight hypervisor that manages memory and CPU allocation and delivers performance that is indistinguishable from bare metal.
- Nitro security chip, integrated into the motherboard.
- The Nitro Cards are a family of cards that offloads and accelerates IO for functions: Nitro Card for VPC, Nitro Card for EBS, Nitro Card for Instance Storage, Nitro Card Controller, and Nitro Security Chip.

## Instance Lifecycle:
- Launch:
	- Creation of an instance from an AMI.
	- Status goes to Pending and then Running.
- Stop: 
	- The instance is stopped.
	- Status goes to Stopping and then Stopped.
	- EBS-backed instances only.
- Start: 
	- Status goes from Stopped to Pending to Running.
	- Each time you transition an instance from stopped to running, we charge a full instance hour, even if these transitions happen multiple times within a single hour.
- Reboot: 
	- Equivalent to an operating system reboot.
	- The instance keeps its public DNS name (IPv4), private IPv4 address, IPv6 address (if applicable), and any data on its instance store volumes. 
- Hibernate:
	- Signal the operating system to perform hibernation (suspend-to-disk).
	- EBS-backed instances only.
- Terminate: 
	- Status goes to Shutting-down and then to Terminated.
	- Each Amazon EBS volume supports the DeleteOnTermination attribute, which controls whether the volume is deleted or preserved when you terminate the instance it is attached to.
	- The default is to delete the root device volume and preserve any other EBS volumes. 
	- You can set a Termination Protection option: prevents a manual termination but does not prevent a termination by Auto Scale in case of a scale-in or in case of a failed health check.

## EC2 Storage:
- Boot volume (disk) can be based on either the Instance Store or EBS.
- Instance Store volume: 
	- volume on the local disk of the host server.
	- Lower latency
	- No data consistency after the VM is terminated (but is consistent on reboot).
	- Not resizeable.
	- AWS does not encrypt these volumes. You need to use OS-level or Filesystem-level encryption.
	- No data redundancy.
- EBS volume:
	- Volume mapped to an EBS volume.
	- Consistent even when the EC2 instance is stopped/terminated.
	- You can change size and type.
	- For both root and data volumes on EBS, to attach the volumes to an instance, these need to be in the same AZ as the instance.
	- Volumes can be encrypted by EBS.
	- Data is written to multiple copies in the same AZ.

## EC2 AMIs:
- Amazon Machine Image.
- Contains the image of the OS and all other software and configurations that you would like when launching new instances.
- Two types of AMIs: EBS-backed or instance-store-backed.
- EBS-backed AMI:
	- Includes one or more EBS snapshots.
	- Built from a snapshot of another EC2 instance after it has been customized.
	- Boot time: usually less than 1mn.
	- Max boot volume size: 16 TiB.
	- You are charged for storing the Snapshot in EBS.
	- The instance type, kernel, RAM disk, and user data can be changed while the instance is stopped. 
- Instance-store-backed AMI:
	- The root device is an instance store volume created from a template stored in Amazon S3. 
	- To build this kind of AMI, you start from an instance that you've launched from an existing instance store-backed Linux AMI. After you've customized the instance to suit your needs, bundle the volume and register a new AMI using AMI tools.
	- Boot time: usually less than 5mn.
	- Max boot volume size: 10 GiB. 
	- You are charged for storing the AMI in S3.
	- Instance attributes are fixed for the life of an instance.
- AMIs include a block device mapping that specifies the volumes to attach to the instance when it's launched.
- AMI IDs are different between regions.
- You can launch instances from an AMI if the AMI owner grants you launch permissions.
- You can copy an AMI within the same AWS Region or to different AWS Regions. 
- When you no longer require an AMI, you can deregister it.
- Deregistering the AMI frees up the EBS snapshot or S3 template that was used to create it. You can then delete the snapshot/template to stop incurring storage costs.
- You can use AMIs from:
	- Amazon provided AMIs: free for Linux AMIs.
	- Marketplace: additional fees paid to the provider.
	- Community AMIs: free to use.
	- You own AMIs.


## EC2 Instance Billing Types:
- On-demand Instances.
- Savings Plans.
- Reserved Instance (RI).
- Spot instances.
- Dedicated Host.
- Dedicated Instance.
- Capacity Reservation.

### On-Demand Instance:
- pay by the second.
- vCPU-based limits per account and per region.
- Instance usages are billed for any time your instances are in a "running" state. 

### Saving Plans:
- commitment to a consistent amount of usage, in USD per hour, for a term of 1 or 3 years.

### Reserved Instance (RI): 
- Is a billing benefit and not an instance type.
- When AWS finds an on-demand instance matching your reservation attributes, it applies the reservation rates to this running instance.
- Payment can be upfront or monthly payment or a mix. The more you pay upfront for your reservation, the greater is your discount.
- Standard RI: 
	- A 1- or 3-yr commitment for a consistent instance configuration, including instance type and Region.
	- Can be sold in the « Reserved Instance Marketplace ».
- Convertible RI:
	- Like Standard RI but you can exchange the reservation for an equal or higher reservation.
	- Reservation change is based on the normalization factor of each instance size (relative to the « small » instance size).
- Scheduled RI: 1-yr commitment, pre-defined schedule (Deprecated).

Regional RIs:
- Reservations are floating in the region.
- Does not reserve capacity.
- Size change supported for Linux.

Zonal RI:
- tied to a specific AZ.
- Reserves capacity in the specified AZ.
- You can change AZ within the same region.
- Instance size change not supported.

## EC2 Spot Instances
- AWS unused capacity.
- up to a 90% discount compared to On-Demand prices. 
- You specify the maximum price you are willing to pay and AWS will allocate Spot instances to you whenever the Spot price is lower than this maximum price your specified. 
- If the Spot price exceeds your maximum willingness to pay for a given instance or when capacity is no longer available, your instance will be removed automatically.
- Removal options: Stop, Hibernate, Terminate.
- Termination notices are issued 2 minutes prior to removal (except for hibernation). 
- When requesting a spot instance you can make a "Persistent Request" so that AWS keeps trying to spin your instance after it is removed.

### Spot Fleet:
- A collection, or fleet, of Spot Instances, and optionally On-Demand Instances. 
- Attempts to launch the number of Spot Instances and On-Demand Instances to meet the target capacity that you specified in the Spot Fleet request.
- Attempts to maintain its target capacity fleet if your Spot Instances are interrupted.
- When Spot Fleet attempts to fulfill your On-Demand capacity, and you have defined multiple on-demand instance types, it will try to launch the lowest-priced instance type first. You can alter this by defining a custom priority (allocation strategy).
- Spot Fleet - set of spot instances + (optional) on-demand instances
- The spot fleet will try to meet the target capacity with price constraints
- A launch pool can have the following can have different instance types, OS, AZ
- We can have multiple launch pools, so the fleet can choose the best
- Spot fleet will stop launching instances the target capacity is reached
- Strategies to allocate spot instances:
    - **lowestPrice**: the spot fleet will launch instances from the pool with the lowest price
    - **diversified**: distribute instances across all pools
    - **capacityOptimized**: launch instances based on the optimal capacity for the number of instances
- Spot fleets allow us to automatically request spot instances with the lowest price

### Spot blocks:
- Spot Instances with a defined duration.
- designed not to be interrupted and will run continuously for the duration you select. 
- You can use a duration of 1, 2, 3, 4, 5, or 6 hours.
- The price that you pay depends on the specified duration. 
- In rare situations, Spot blocks may be interrupted due to Amazon EC2 capacity needs. You are not billed for interrupted instances.

## Dedicated Hosts:
- A physical server with EC2 instance capacity fully dedicated to your use. 
- Allow you to use your existing per-socket, per-core, or per-VM software licenses (BYOL).

Dedicated Instance:
- For instances that need to run on single-tenant hardware. 
- May share the host with other instances from the same AWS account. 
- You have no visibility on the host.
- You have no guarantee that when you run the instance it will always start on the same host.
- BYOL not supported.

On-Demand Capacity Reservations:
- Enable you to reserve capacity for your Amazon EC2 instances in a specific Availability Zone for any duration. 
- Billing starts as soon as the capacity is provisioned.
- When you no longer need it, cancel the Capacity Reservation to stop incurring charges. 
- You specify: AZ, number of instances, instance attributes.
- Can be combined with Savings Plans or Regional Reserved Instances to receive a discount. 

## EC2 Auto Scaling:
- Ensures that you have the correct number of Amazon EC2 instances available to handle the load for your application. 
- Instances are grouped in Auto Scaling Groups (ASG).
- Auto Scaling does basically two things:
	- Automatically replaces instances that are unhealthy.
	- Automatically scales out or scales in your ASG to meet your scaling policy.
- ASGs are Region-wide.
- Number of instances in ASG:
	- You specify a min, a max and a desired number of instances in an ASG.
	- Desired number: the number of instances launched at start.
	- The desired number can be changed manually (manual scaling) or by auto scaling.
	- Min and Max number: boundaries for all requests to change the desired number (either manual or auto scaling).
- Launch templates & Launch configurations: 
	- Configuration template for your instances (AMI ID, instance type, key pair, security groups, and block device mapping, …).
	- Launch Configurations cannot be modified (you can only delete or copy). Launch Templates can be modified (a new version is created each time a template is modified).
	- Launch templates support more instance types and offers more options than launch configurations.
	- An ASG can have multiple launch templates.
	- You can assign weight to instance types and define the mix you want between on-demand and Spot instances.
- Supports health check notifications. Can come from EC2, ELB, or a custom health check. Unhealthy instances are replaced.
- Scaling policy types:
	- Fixed number.
	- Manual.
	- Scheduled.
	- Simple scaling & Step scaling: step adjustments based on threshold values for the CloudWatch alarms. The difference is that with Step scaling, the adjustments vary based on the size of the alarm breach, which is not the case with simple scaling.
	- Target Tracking: you select a load metric for your application, such as CPU utilization or request count, set the target value, and Amazon EC2 Auto Scaling adjusts the number of EC2 instances in your ASG as needed to maintain that target (like a thermostat).
	- Scaling based on SQS: you can build a scaling policy that responds to the load of an SQS queue by defining a custom CloudWatch metric and defining a target tracking scaling policy.
- Supports time to wait for new instances to warm-up: while scaling out, Auto Scaling does not consider instances that are warming up as part of the current capacity of the group. 
- Cool down period: 
	- Specific to Simple Scaling policies.
	- After the Auto Scaling group scales, it waits for a cooldown period to complete before any further scaling activities initiated by simple scaling policies can start.
	- During the cool down period, there is no impact on other policy types. For example, a target scaling policy can trigger during the cool down period.
	- Default cool down is 300 seconds.
- Lifecycle hooks:
	- Let you take action before an instance goes into service or before it gets terminated. 
	- The instances are marked "paused" as an Auto Scaling group launches or terminates them.
	- When an instance is "paused", it remains in a wait state either until you complete the lifecycle action using the complete-lifecycle-action command or the CompleteLifecycleAction operation, or until the timeout period ends (one hour by default). 

Auto Scaling Instance Termination policy:
- First identifies which of the two types (Spot or On-Demand) should be terminated.
- Default policy: designed to help ensure that your instances span Availability Zones evenly for high availability. Then selects the instance that uses the oldest launch template or configuration.
- Can be customized.
- To prevent EC2 Auto Scaling from terminating a particular instance when scaling in, use instance scale-in protection on the instance or on the ASG. Does not prevent termination from the console nor when unhealthy instances are replaced.
- To prevent EC2 Auto Scaling from terminating unhealthy instances, suspend the ReplaceUnhealthy process.

Auto Scaling with ELB:
- You can attach the load balancer to your Auto Scaling group to register the group with the load balancer. 
- Instances that are launched/terminated by your Auto Scaling group are automatically registered/deregistered with the load balancer. 
- You can configure your Auto Scaling group to use ELB metrics such as the ALB request count per target (or other metrics) to scale the number of instances in the group.
- You can also optionally enable Auto Scaling to replace instances based on health checks provided by the ELB.

## Other Features:
- Elastic Fabric Adapter: fast network interface for HPC (similar to Infiniband).

### Virtualization types:
- Linux Amazon Machine Images use one of two types of virtualization: paravirtual (PV) or hardware virtual machine (HVM). 
- The main differences between PV and HVM AMIs are the way in which they boot and whether they can take advantage of special hardware extensions (CPU, network, and storage) for better performance.
- For the best performance, we recommend that you use current generation instance types and HVM AMIs when you launch your instances. 

### Key Pair:
- consisting of a private key and a public key,
- used to prove your identity when connecting to an instance.
- When creating an instance, you can generate a new key pair or use an existing one.
- Amazon EC2 stores the public key, and you store the private key. 
- On Linux: the key pair file is used for SSH connections.
- On Windows: the key pair is used when you click « Get Administrator Password » on the instance.

## Placement Groups:
- When you launch a new EC2 instance, the EC2 service attempts to place the instance in such a way that all of your instances are spread out across underlying hardware to minimize correlated failures.
- You can use placement groups to influence the placement of a group of interdependent instances to meet the needs of your workload. 
- Placement strategies: Cluster, Partition, Spread.

#### Cluster Placement Groups

- Used for highest possible performance
- Best practice is to launch all of the instances at the same time which will be part of the placement group. This ensures that AWS allocates capacity in the same location
- Cluster placement groups are located in the same AZ, when the first instance is launched, the AZ is locked
- Ideally the instances in a cluster placement group are located on the same rack, often on the same EC2 host
- All instances have fast bandwidth between each other (max 10Gbps vs 5Gbps which can be achieved normally)
- They offer the lowest latency possible and max PPS possible in AWS
- To achieve these levels of performance we need to use instances with high performant networking: instances with more bandwidth and with Enhanced Networking
- Cluster placement groups should be used for highest performance. They offer no HA and very little resilience
- Considerations for cluster placement groups:
    - We can not span AZs, the AZ is locked when the first instance is launching
    - We can span VPC peers, but this will impact performance negatively
    - Cluster placement groups are not supported for every instance type
    - *Recommended*: use the same type of instances and launch them at the same time
    - Cluster placement groups offer 10 Gbps for single stream performance

#### Spread Placement Groups

- They offer the maximum possible availability and resiliency
- They can span multiple AZs
- Instances in the same spread placement group are located on different racks, having isolated networking and power supplies
- There is a limit for 7 instances per AZ in case of spread placement groups
- Considerations:
    - Spread placement provides infrastructure isolation
    - Hard limit: 7 instances per AZ
    - We can not use dedicated instances or hosts

#### Partition Placement Groups

- Similar to spread placement groups
- They are designed for situations when we need more than 7 instances per AZ but we still need separation
- Can be created across multiple AZs in a region
- At creation we specify the number of partition per AZ (max 7 per AZ)
- Each partition has its own rack with isolated power and networking
- We can launch as many instances as we need in a partition group
- Use cases for partition groups: HDFS, HBase, Cassandra, topology aware applications
- Instances can be placed in a specific partition or we can let AWS to decide
- An instance can belong to only one placement group.
- Existing instances cannot be moved into a placement group.
- PGs cannot be merged.
- Can contain reserved instances but you cannot reserve capacity for the group itself.
- Dedicated Hosts not supported.

### Data Transfer Costs:
- Data transferred "in" to and "out" from EC2 and ElastiCache instances or Elastic Network Interfaces across Availability Zones or VPC Peering connections in the same AWS Region is charged at $0.01/GB in each direction.
- Same AZ: free
- Data transferred between S3/Glacier/DynamoDB/SES/SQS/Kinesis/ECR/SNS/SimpleDB and Amazon EC2 instances in the same AWS Region is free. 


### EC2 Instance metadata and user data:
- Data attached to your instance and that you can access from within your instance.
- Can be used to configure or manage the running instance.
- Instance metadata:
	- AWS metadata about your instance.
	- Divided into categories, for example, host name, events, and security groups. 
- User data:
	- Data that you specified when launching your instance.
	- For example, you can specify parameters for configuring your instance, or include a simple script. 
	- Can be passed as text, base64 or file.
	- If the text starts with a shebang (#!) it will be executed after the instance in launched.
- EC2 instances can also include dynamic data, such as an instance identity document that is generated when the instance is launched. 
- Metadata security: although you can only access instance metadata and user data from within the instance itself, the data is not protected by authentication or cryptographic methods. Any software running on the instance, can view its metadata. 
- Metadata can be retrieved using the "Instance MetaData Service": IMDSv1 or IMDSv2. Based on HTTP requests.
- IMDSv2 uses an authentication token: the first request retrieves a token that can be used for up to 6 hours.
- Example: curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/
- The IP address 169.254.169.254 is a link-local address and is valid only from the instance.