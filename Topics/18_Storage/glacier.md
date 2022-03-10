# S3 Glacier:
- extremely low-cost Amazon S3 storage class for data archiving and long-term backup. 
- REST-based web service. 
- a vault is a container for storing archives (any type of files).
- up to 1,000 vaults per account.
- unlimited number of archives in a vault.
- Archives can be up to 40 TB.
- Integrates with S3 lifecycle policies.
- Query in Place supported with S3 Glacier Select. 
- Retrieving an archive from S3 Glacier is an asynchronous 2-step operation:
	- You first initiate a retrieval job,
	- After the job completes, you send a GET request to download the output. 
- 3 retrieval options for Glacier:
	- Expedited: 1-5 minutes.
	- Standard: 3-5 hours.
	- Bulk: 5-12 hours.
- 2 retrieval options for Glacier Deep Archive:
	- Standard: 12 hours.
	- Bulk: 24 hours.
- Supports retrieval policies (limits).
- Supports a Vault Lock policy.
- All data is encrypted in Glacier.


S3 Provisioned Capacity Units:
- Provisioned Capacity guarantees that your retrieval capacity for Expedited retrievals will be available when you need it. 
- Each unit of capacity ensures that at least 3 expedited retrievals can be performed every 5 minutes and provides up to 150MB/s of retrieval throughput. 
- Without provisioned capacity, Expedited retrieval requests will be accepted if capacity is available at the time the request is made. 
