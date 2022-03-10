# Route 53:
- Worldwide distributed DNS with 100% SLA.
- Is also a registrar for your domain name.
- Has an API.
- Can do server health checks.
- Two types of zones: 
	- Public hosted zone: visible on internet.
	- Private hosted zone: used only from within your VPCs. You need to enable « DNS Resolution » and « DNS Hostnames » on the VPC.
- Refer to the VPC notes for more information about private hosted zones.
- You can associate any DNS record type except SOA and NS records.

## Route 53 Records:
- Can be standard DNS records or Route 53 "Alias" records.
- To build routing policies you create multiple records of the same name and type and pointing to different IP addresses. Eg: [ { myrecordname=IP1, policy=weighted, weight=40 }, { myrecordname=IP2, policy=weighted, weight=60 } ].
- You create a DNS record and then associate a health check with this record. 
- Records without a health check are assumed always healthy.
- Unhealthy records are removed from the routing algorithm.

DNS standard record types:
- A: IPv4 address for a DNS name.
- AAAA: IPv6 address for a DNS name.
- CNAME: Alias (Canonical name)
- MX: email
- TXT
- SRV
- SPF: Sender Policy Framework: IP address of email gateway to avoid spoofing.
- NS: name server record.
- SOA: Start of authority.

Route 53 "Alias" Records:
- Provide a Route 53-specific extension to DNS functionality. 
- Points either:
	- to an AWS Service, such as CloudFront, S3, ELB, API Gateway, VPC interface endpoint, …etc.
	- to another record. Enables you to build complex trees of routing rules.
- Target IPs are automatically maintained by Route 53.
- "Evaluate Target Health" option:
	- Indicates whether Route 53 should check the health of the target resource that this alias points to.
	- You can set this option in both the case of target AWS service and the case of another record.
	- Cannot be set when the Alias points to a CloudFront distribution.
- Support Apex names.

## Route 53 Health Checks:
- Optional construct that you can attach to a record.
- Can monitor an Endpoint, Other Health Checks or a CloudWatch alarm state (a CloudWatch alarm can have a state of "OK", "In-Alarm" or "Insufficient data").
- Can use TCP, HTTP or HTTPS probes. 
- Can specify port and HTTP(S) path.
- Endpoint:
	- An element in the parameters of the health check. 
	- can be an IP address or domain name. 
	- not necessarily identical to the IP address of the DNS record. Eg: a record pointing to IP1 can be associated with a health check whose endpoint is IP2.
- Health Checks Interval:
	- Can be set to 30 seconds (Standard) or 10 seconds (Fast).
	- Typically, around 15 health checkers from around the world send health checks to your resource.
	- If you choose an interval of 30 seconds, your resource will receive a health check request about every two seconds.
	- If you choose an interval of 10 seconds, your resource will receive a health check request more than once per seconds.
- You can customize the health checkER regions.
- Can search a string in the HTTP response body.
- Can create an alarm to send notifications using Amazon SNS.
- Integrates with ELBs: Route 53 creates and manages the health checks for your ELB automatically if the “Evaluate Target Health” is enabled.
- Supports SNI.

## Route 53 Routing Policies:
- Simple routing policy:
	- Associates an A record with one or more IP addresses. 
	- You can't use multiple records of the same name and type with simple routing. 
	- However, a single record can contain multiple values (such as IP addresses). 
	- In case of multiple IP addresses, the list is returned in a random order. 
- Failover routing policy:
	- Active-passive failover between a primary record and a secondary record. 
	- Both can be single or multiple records.
	- Route 53 does NOT respond to DNS queries using the secondary record until ALL primary records are unhealthy.
	- A popular scenario for web sites is to have a failover policy to a static web site in S3.
- Geolocation routing policy:
	- Based on the location of your users: by continent, by country or by state (the latter for US only).
	- Does not necessarily mean the best latency.
- Geoproximity routing policy – Based also on the location of your users. Uses a geoproximity map that you can alter.
- Latency routing policy:
	- Based on latency between the user and the AWS data centers.
	- AWS maintains a database of latencies from different parts of the world.
- Multivalue answer routing policy – Responds to DNS queries with up to eight healthy records selected at random.
- Weighted routing policy – Use to route traffic to multiple resources in proportions that you specify.

## Route 53 Traffic Flow:
- A tool that greatly simplifies the process of creating and maintaining records in large and complex configurations.
- Visual editor: The traffic flow visual editor lets you create complex trees of records and see the relationships among the records. 
- Versioning: You can create multiple versions of a traffic policy so you don't have to start all over when your configuration changes. 
- Automatic record creation and updating:
	- A traffic policy can represent dozens or even hundreds of records. 
	- Traffic flow lets you create all those records automatically by creating a traffic policy. 
	- You specify the hosted zone and the name of the record at the root of the tree, such as example.com or www.example.com, and Route 53 automatically creates all the other records in the tree. 
- The geoproximity routing policy is available only if you use Traffic Flow. 
- There's a monthly charge for each traffic policy record. 

## Route 53 Resolver:
- When you create a VPC, Route 53 Resolver automatically answers DNS queries for local VPC domain names for EC2 instances and for records in private hosted zones.
- You can also integrate DNS resolution between the Route 53 Resolver and your DNS resolvers on "your network" by configuring forwarding rules.
- "Your network" here can include: your VPC, peered VPCs, your on-premises networks connected via S2S VPN or Direct Connect.
- Two use cases: inbound DNS requests and outbound DNS requests.
- Inbound DNS requests:
	- You create an "Inbound endpoint": DNS resolvers on your network can forward DNS queries to the Route 53 Resolver via this endpoint.
	- This allows your DNS resolvers to easily resolve domain names for AWS resources such as EC2 instances or records in a Route 53 private hosted zone. 
- Outbound DNS requests:
	- You create an "Outbound endpoint": the Route 53 Resolver conditionally forwards queries to DNS resolvers on your network via this endpoint.
	- You create Resolver rules that specify the domain names for the DNS queries that you want to forward, and the IP addresses of the DNS resolvers on your network that you want to forward the queries to. 
	- If a query matches multiple rules, Resolver chooses the rule with the most specific match.
	- You can also forward queries for amazonaws.com to your network. This rule will not apply to all amazonaws.com subdomains.
- For each endpoint, Resolver automatically creates a VPC elastic network interface with an IP address that you specify.
- Like Amazon VPC, Resolver is regional. 