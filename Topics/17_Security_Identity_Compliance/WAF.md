# AWS WAF:
- Web Application Firewall for CloudFront, API Gateway, Application Load Balancer, or AppSync GraphQL API. 
- Web ACL: a set of rules in AWS WAF to protect your AWS resources.
- Each rule requires at least one top-level statement, which might contain nested statements at any depth, depending on the rule and statement type. 
- Statements can be combined using AND, OR or NOT.
- Examples of criteria in the statements:
	- Scripts or SQL code that are likely to be malicious, like cross-site scripting (XSS) or SQL injection.
	- IP addresses or address ranges that requests originate from.
	- Country or geographical location that requests originate from.
	- Strings that appear in the request, like in the User-Agent header or the query string. Supports use of regular expressions (regex).
	- Request size limit.
- Each rule contains action to take if a web request meets the criteria:
	- Count. You can insert custom headers into the request and you can add labels that other rules can match against. 
	- Allow. You can insert custom headers into the request before forwarding it to the protected resource. 
	- Block. You can customize the response. Default response is HTTP 403 (Forbidden).
- Labels:
	- A label is metadata that a rule can add to matching web requests. 
	- Rules can also match against labels when they inspect web requests.
	- Labels allow a matching rule to communicate results to the rules that are evaluated later in the same web ACL. 
- You can use rules individually or in reusable rule groups. 
- AWS Managed Rules and AWS Marketplace sellers provide managed rule groups for your use. 
- You can add a scope-down statement inside some rules: narrows the scope of the requests that the rule evaluates. Can be also applied to rate-based rules.
- Bot Control: helps you manage bot activity to your site by categorizing and identifying common bots.
- For a CloudFront distribution, AWS WAF is available globally, but you must use the region US East (N. Virginia) for all of your work. 
- Uses web ACL capacity units (WCU) to calculate and control the operating resources that are required to run your rules, rule groups, and web ACLs. 

- Comes in 2 versions:
    - **Shield Standard**:
        - Offers protection against all known infrastructure Layer 3 and Layer 4 DDoS attacks
        - It is free of charge
        - If offers comprehensive L3 and L4 protection when used with Route53 and CloudFront
    - **Shield Advanced**:
        - Costs $3000 per month per organization
        - Expands the range of products which can be protected: EC2, ELB, CloudFront, Global Accelerator and Route53
        - Shield Advanced provides access to 24/7 advanced response team
        - Provides financial insurance for any increase of payments in case of DDoS attacks

## WAF - Web Application Firewall

- It is Layer 7 Firewall (understands HTTP/S)
- Normally firewall operate at Layer 3, 4, 5
- WAF protects against complex Layer 7 attacks/exploits such as SQL Injection, Cross-Site Scripting
- It can filter based on location (Geo Blocks), and provides rate awareness
- Web Access Control List (WEBACL) integrated with ALB, API Gateway and CloudFront
- WEBACL has rules and they are evaluated when traffic arrives