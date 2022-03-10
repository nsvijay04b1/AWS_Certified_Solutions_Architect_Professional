# AWS CloudFront
- CDN using a global network of 225+ edge locations.
- Two levels of edge locations:
	- Point-Of-Presence (POP): close to the end user (low latency). 
	- Regional Edge Caches: higher level cache. Have a much larger caching capacity than POPs. 
- Supports static, dynamic, interactive and streaming content.
- Common video formats are MPEG DASH, Apple HLS, Microsoft Smooth Streaming, and CMAF. 
- Supports WebSockets also.
- You create a CloudFront distribution to tell CloudFront how to distribute your content.
- A Distribution defines:
	- Delivery methods: Web, VoD, Live Streaming.
	- You content origin: S3 bucket, EC2 and ELB or any HTTP service.
	- Access: open or restricted.
	- Security: HTTP or HTTPS.
	- Cache key.
	- Origin request settings: parameters of the request sent by CloudFront to your content origin. 
	- Geo-restrictions: prevent users in selected countries from accessing your content. Can be an allow-list or a deny-list.
- For RTMP distributions, you must store your content on S3.
- Cache Key:
	- Uniquely identifies each file in the cache for a given distribution. 
	- Determines whether a viewer request results in a cache hit.
	- Values included in the cache key can be HTTP request query strings, headers, and cookies. 
	- By default CloudFront does not consider cookies nor headers. You can change this.
- Origin Requests:
	- These are the HTTP requests sent by CloudFront to the content origin when you have a cache miss.
	- Even when you want to cache based on specified query parameters, CloudFront will forward the whole query string to the origin.
	- All HTTP headers and cookies that you include in the cache key are also automatically included in origin requests. 
	- CloudFront can add URL query parameters and HTTP headers to origin requests. Eg: CloudFront-Viewer-Country header.
- Policies:
	- control the cache key, the origin request and the HTTP headers.
	- For origin requests, you can use managed policies, or you can create your own.
- Cache expiration:
	- By default, each file automatically expires after 24 hours.
	- To change the cache duration for all files that match the same path pattern, you can change the CloudFront settings for Minimum TTL, Maximum TTL, and Default TTL.
	- To change the cache duration for an individual file, you can configure your origin to add a Cache-Control max-age or Cache-Control s-maxage directive, or an Expires header field to the file.
	- Cache-Control s-maxage directive: consumed by CloudFront only.
	- Cache-Control max-age directive: consumed by the browser. Takes precedence on the « Expires » directive. Consumed also by CloudFront if « Cache-Control s-maxage » is not specified.
	- In case both Maximum TTL and max-age are set, CloudFront caches objects for the lesser of the two values (AND operator). 
- If you need to remove a file from CloudFront edge caches before it expires, you can do one of the following:
	- Invalidate the file from edge caches. The next time a viewer requests the file, CloudFront returns to the origin to fetch the latest version of the file.
	- Use file versioning to serve a different version of the file that has a different name. This is the recommended method.
- Origin Failover capability: automatically serves content from a backup origin when the primary origin is unavailable. 
- Origin Access Identity (OAI) feature: OAI is a special CloudFront user to which you grant access to your S3 bucket. You then deny direct access to your S3 bucket to all users except the OAI.
- S3 bucket recommended access format: bucket-name.s3.region.amazonaws.com.
- Restricting access to files in CloudFront:
	- You can configure CloudFront to require that users access your files using either signed URLs or signed cookies.
	- Uses RSA-SHA1 for signing URLs or cookies. 
	- in the signed URL/cookie you can include the validity period of the access (start & end) and the valid IP addresses or range.
	- Use signed cookies if you don't want to change your current URLs or to give selected users access to all files.
	- Signed URLs take precedence over signed cookies. 
	- The signed URL/cookie is created by your application. You need therefore to specify in CloudFront the "trusted key groups" or "trusted signers" that you want to use to create this signed URL/cookie. 
- You can set up AWS WAF in front of CloudFront.
- Price Class for edge locations:
	- Default price class: all regions worldwide.
	- Most regions class (excludes the most expensive regions like South America).
	- Least expensive regions class: US, Canada and Europe.


# Domain Names and SSL Certificates:
- When you create a web distribution, CloudFront assigns a domain name to the distribution, such as d111111abcdef8.cloudfront.net. 
- If you want to use your own domain name, you can create a DNS record that points to your CloudFront distribution:
	- if using Route 53, create a Route 53 Alias record.
	- if using another DNS provider, create a CNAME record.
- SSL Certificates:
	- For the CloudFront domain name, you can use the default certificate.
	- For custom domain names, you can use certificates from ACM or IAM. The certificate must be in the us-east-1 region (US East N. Virginia).
- Supports Server Name Indication (SNI). Default and free option.


# AWS Lambda@Edge:
- Run Lambda functions at the CloudFront locations.
- Lets you execute functions that customize the content that CloudFront delivers. 
- You can author Node.js or Python functions in one Region, US-East-1 (N. Virginia), and then execute them in AWS locations globally that are closer to the viewer, without provisioning or managing servers.
- You can execute Lambda functions when the following CloudFront events occur:
	- When CloudFront receives a request from a viewer (viewer request)
	- Before CloudFront forwards a request to the origin (origin request)
	- When CloudFront receives a response from the origin (origin response)
	- Before CloudFront returns the response to the viewer (viewer response)

CloudFront logging: Two ways to log the requests that come to your distributions:
- Standard logs (access logs):
	- Provide detailed records about every request that’s made to a distribution.
	- Delivered to the Amazon S3 bucket of your choice.
	- Free but you pay for the S3 storage.
- Real-time logs:
	- Provide sampled information about requests made to a distribution,
	- Delivered in near real time (within seconds).
	- You can choose the sampling rate,
	- You can also choose the specific fields to log.
	- Delivered to the data stream of your choice in Amazon Kinesis Data Streams.
	- CloudFront charges for real-time logs, in addition to the charges you incur for using Kinesis Data Streams. 

# CloudFront VOD:
- To deliver video on demand (VOD) streaming with CloudFront, use the following services:
	- An encoder (such as AWS Elemental MediaConvert) to transcode the video into streaming formats.
	- Amazon S3 to store the content in its original format and to store the transcoded video.
	- CloudFront to deliver the transcoded video to viewers. 
- Transcode your content by using a MediaConvert job. The job converts your video into the formats required by the players that your viewers use.


Live Streaming Video with CloudFront and AWS Media Services:
- Use AWS Elemental MediaLive to encode live video streams in real time. 
- After you compress a live video stream, you can use either of the following two main options to prepare and serve the content:
	a- Convert your content into required formats, and then serve it. You can use AWS Elemental MediaPackage to package the content for different device types.
	b- Store and serve your content using scalable origin: If MediaLive encoded content in the formats required by all of the devices that your viewers use, use a highly scalable origin like AWS Elemental MediaStore to serve the content. 
