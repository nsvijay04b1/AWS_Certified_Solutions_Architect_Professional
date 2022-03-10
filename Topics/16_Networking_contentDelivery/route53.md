# Route53

## DNS Fundamentals

- DNS is a discovery service, translate information which machines need into information that humans need and vice-versa
- Example: www.amazon.com => 104.98.34.131
- DNS database is a huge distributed database
- DNS allows a DNS resolver server to find a Zone File ona Name Sever (NS) and query the it, retrieving the necessary IP address for a DNS name

## DNS Terminology

- **DNS Client**: refers to a customer PC, laptop, tablet, etc.
- **DNS Resolver**: software running on a device or a server which queries DNS on our behalf
- **DNS Zone**: a part of the DNS database (example: amazon.com)
- **Zone file**: it is a physical database for a zone, contains all the DNS information for a particular domain
- **DNS Name server**: a server which hosts the zone files

## DNS Root

- DNS is structures like a tree, DNS root is at the top of the tree
- DNS root is hosted in 13 special nameservers, known as DNS Root Servers
- Root hints file: an OS file containing the address of all root servers
- Authority: when something is trusted in DNS, example root hints file
- Delegation: trusted authorities can delegate a part of themselves to other entities, those entities becoming authoritative for the part delegated

## Route53 Introduction

- It is a managed DNS product
- Provides 2 main services:
    - Register domains
    - Can host zone files on managed nameservers
- It is a global service, its database is distributed globally and resilient
- Worldwide distributed DNS with 100% SLA.
- Is also a registrar for your domain name.
- Has an API.
- Can do server health checks.
- Two types of zones: 
	- Public hosted zone: visible on internet.
	- Private hosted zone: used only from within your VPCs. You need to enable « DNS Resolution » and « DNS Hostnames » on the VPC.
- Refer to the VPC notes for more information about private hosted zones.
- You can associate any DNS record type except SOA and NS records.

## Hosted Zones

- DNS as a service
- Let's us create and manage zone files
- Zone files are hosted on four managed nameservers
- A hosted zone can be public, accessible for the public internet, part of the public DNS system
- It can be private, linked to a VPC(s)
- A hosted zone stores DNS records (recordsets)


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

### DNS TTL - Time To Live

- Indicates how long records can be cached for
- Resolver server will store the records for the amount of time specified by the TTL in seconds
- TTL is a balance: low values means less queries to the server, high values mean less flexibility when the records are changed

### Route53 Public Hosted Zones

- A hosted zone is DNS database for a given section of the global DNS database
- Hosted zones are created automatically when a domain is registered in R53, they can be created separately as well
- There is a monthly fee for each running hosted zone
- A hosted zone hosts DNS records, example A, AAAA, MX, NS, TXT etc.
- Hosted zones are authoritative for a domain
- When a public hosted zone is created, R53 allocates 4 public name servers for it, on these name servers the zone file is hosted
- We use NS records to point at these name servers to be able to connect to the global DNS
- Externally registered domains can point to R53 public zone

### Route53 Private Hosted Zones

- Similar to a public hosted zone except it can not be accessed from the public internet
- It is associated with VPCs from AWS and it only can be accessed from VPCs from the account. Cross account access is possible
- Split-view: it is possible to create split-view (split-horizon) DNS for public and internal use with the same zone name. Useful for accessing systems from the private network without accessing the public internet

### CNAME vs ALIAS Records

- The problem with CNAME:
    - An A record maps a NAME to an IP Address
    - A CNAME records maps a NAME to another NAME
    - We can not have a CNAME record for an APEX/naked domain name
    - Many AWS services use DNS Name (example: ELBs)
- For the APEX domain to point to another domain, we can use ALIAS records
- An ALIAS record maps a NAME to an AWS resource
- Can be used for both apex and normal records
- There is no additional charge for ALIAS requests pointing at AWS resources
- An alias is a subtype, we can have an A record alias and a CNAME record alias

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

#### Route53 Failover Routing

- We can add 2 records of the same name (a primary and a secondary)
- Health checks happen on the primary record
- If the primary record fails the health checks, the address of the secondary record is returned
- Failover routing should be used when we configure active-passive failover

#### Route53 Multi Value Routing

- Multi Value Routing is mixture to simple and failover routing
- With multi value routing we can create many records with the same name
- Each record can have an associated health check
- When queried, 8 records are returned to the client. If more than 8 records are present, 8 records will be randomly selected and returned
- The client picks one a of the values and connects to the service
- Any records which fails the health checks won't be returned when queried
- Multi value routing is not a substitute for an actual load balancer

#### Route53 Weighted Routing

- Weighted Routing can be used when looking for a simple form of load balancing or when we want to test new versions of an API
- With weighted routing we can specify a weight for each record
- For a given name the total of weight is calculated. Each record is returned depending on the percentage of the record compared to the total weight
- Setting a weight to 0, the record will not be returned
- Weighted routing can be combined with health checks. Health checks don't remove records from the calculation of the total weight. If a record is selected, but it is unhealthy, another selection will be made until a healthy record is selected

#### Route53 Latency-Based Routing

- Should be used when we trying to optimize for performance and better user experience
- For each record we can specify an region
- AWS maintains a list of latencies for each region from the world (source - destination latency)
- When a request comes in, it will be directed to the lowest latency destination based on the location from where the request is coming from
- Latency-based routing can be combined with health checks. If the lowest latency record fails, the second lowest latency record will be returned to the client
- The database maintained by AWS is not updated in real time

#### Route53 Geolocation Routing

- Geolocation routing is similar to latency-based routing, only instead of latency the location of customer and resources is used to determine the resolution decisions
- When we create records, we tag the records with a location
- This location is generally a country (ISO2 code), continent or default. There can be also a subdivision, in USA we can tag records with a state
- When the user does a query, an IP checks verifies the location is the user
- Geolocation does not return the closest record, it returns relevant records: when the resolution happens, the location of the user is cross-checked with the location specified for the records and the matching on is returned
- Order of the cross-checks is the following:
    1. R53 checks the state (US only)
    2. R53 checks the country
    3. R53 checks the continent
    4. Returns default if not previous match
- If no match is detected, the a `NO ANSWER` is returned
- Geolocation is ideal for restricting content based on the location of the user
- It can be used for load-balancing based on user location as well

#### Route53 Geoproximity Routing

- Geoproximity aims to provide records as close to the customer as possible, aims to calculated the distance between to record and customer and return the record with the lower one
- When using geoproximity, we define rules:
    - Region the resource is created in if it is an AWS resource
    - Lat/lon coordinate for external resources
    - Bias
- Geoproximiy allows defining a bias: it can be a `+` or `-` bias, increasing or decreasing the region size. We can influence the routing distance based on this bias

#### Simple routing policy:
- Associates an A record with one or more IP addresses. 
- You can't use multiple records of the same name and type with simple routing. 
- However, a single record can contain multiple values (such as IP addresses). 
- In case of multiple IP addresses, the list is returned in a random order. 


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

## Route53 Interoperability

- Route53 acts as a domain registrar and as a domain hosting
- We can also register domains using other external services
- Steps happening when we register a domain using R53:
    - R53 accepts the registration fee
    - Allocates 4 Name Servers
    - Creates a zone file (domain hosting) on the NS
    - R53 communicates with the registry of the top level domain and adds the address of the 4 NS for the given domain
- Route53 acting as a registrar only:
    - We pay for the domain for Route53 but the name servers are allocated by other entity
    - We have to allocate the name servers to Route53 which will communicate with the top level domain registry
- Using Route53 for hosting only:
    - Generally used for existing domains. The domain is registered at third party
    - We create a hosted zone inside R53 and provide the address of the name servers to the third party
