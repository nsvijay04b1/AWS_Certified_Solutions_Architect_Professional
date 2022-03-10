## Cost Explorer

- Tracks and analyzes your AWS usage. It is free for all accounts
- Includes a default report that helps visualize the costs and usage associated with our TOP FIVE cost-accruing AWS services, and gives you a detailed breakdown on all services in the table view
- We can view data for up to last 12 months, forecast how much we are likely to spend for the next tree months and get recommendations on what Reserved Instances to purchase
- Cost Explorer must be enabled before it can be used. The owner of the account can enable it

## AWS Cost and Usage Reports

-  AWS Cost and Usage report provides information about our usage of AWS resources and estimated costs for that usage
- The AWS Cost and Usage report is a `.csv` file or a collection of `.csv` files that is stored in an S3 bucket. Anyone who has permissions to access the specified S3 bucket can see the billing report files
- We can use the Cost and Usage report to track your Reserved Instance Utilization, charges, and allocations
- For time granularity, we can choose one of the following:
    - Hourly: if we want our items in the report to be aggregated by the hour
    - Daily: if we want our items in the report to be aggregated by the day
    - Monthly: if we want our items in the report to be aggregated by month
- Report can be automatically uploaded into AWS Redshift and/or AWS QuickSight for analysis

# Cost Allocation Tags

- Are tags that we can enable to provide additional information for any billing report in AWS
- Cost Allocation Tags needs to enabled individually per account or per organization from the master account
- There 2 different form of Cost Allocation Tags:
    - AWS generated - example: `aws:createdBy` or `aws:cloudformation:stack-name`. These are added automatically by AWS if cost allocation tags are enabled
    - User defined tags: `user:something`
- Both type of tags will be visible in AWS cost reports and can be used as a filter
- Cost Allocation Tags appear only int he Billing Console
- After enabling Cost Allocation Tags, it can take up to 24 hours to be visible and active
- Cost Allocation Tags are not added retroactively