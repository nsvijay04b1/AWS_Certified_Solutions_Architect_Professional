# AWS IoT: 

AWS IOT provides the cloud services that connect your IoT devices to other devices and AWS cloud services. 

IoT Architecture Components:
- Device Software:
	- AWS IoT Greengrass
	- FreeRTOS
	- AWS IoT Device Tester
	- AWS IoT Device SDKs
- Control Services:
	- AWS IoT Device Mgmt
	- AWS IoT Core
	- AWS IoT Device Defender
	- AWS Hot Things Graph
- Data Services:
	- AWS IoT SiteWise
	- AWS IoT Events
	- AWS IoT Analytics

======================================

FreeRTOS:
- FreeRTOS is an open source, real-time operating system for microcontrollers that lets you include small, low-power edge devices in your IoT solution. 
- FreeRTOS includes a kernel and a growing set of software libraries that support many applications. 
- FreeRTOS systems can securely connect your small, low-power devices to AWS IoT and support more powerful edge devices running AWS IoT Greengrass.

AWS IoT Greengrass:
- AWS IoT Greengrass extends AWS to edge devices so they can act locally on the data they generate and use the cloud for management, analytics, and durable storage. 
- With AWS IoT Greengrass, connected devices can run AWS Lambda functions, Docker containers, or both, execute predictions based on machine learning models, keep device data in sync, and communicate with other devices securely – even when they are not connected to the Internet.

AWS IoT Device Tester:
- AWS IoT Device Tester for FreeRTOS and AWS IoT Greengrass is a test automation tool for microcontrollers.
- AWS IoT Device Tester, test your device to determine if it will run FreeRTOS or AWS IoT Greengrass and interoperate with AWS IoT services.

AWS IoT Device SDKs:
- Help you efficiently connect your devices to AWS IoT.
- Include open-source libraries, developer guides with samples, and porting guides so that you can build innovative IoT products or solutions on your choice of hardware platforms.

======================================

AWS IoT Core:
- A managed cloud service that enables connected devices to securely interact with cloud applications and other devices. 
- Can support many devices and messages, and it can process and route those messages to AWS endpoints and other devices. 
- Device gateway: 
	- Enables devices to securely and efficiently communicate with AWS IoT. 
	- Uses X.509 certificates.
- Message Broker:
	- Provides a secure mechanism for devices and AWS IoT applications to publish and receive messages from each other. 
	- Supported protocols: MQTT, MQTT over WebSocket, HTTP, LoRaWAN.
- Rules Engine:
	- Connects data from the message broker to other AWS services for storage and additional processing. 
	- Rules trigger actions.
	- For example, you can insert, update, or query a DynamoDB table or invoke a Lambda function based on an expression that you defined in the Rules engine. 
- IoT data can be either :
	- sent to a message topic that triggers an IoT Rule or,
	- sent directly to an IoT Rule.


AWS IoT Device Management:
- Help you track, monitor, and manage the plethora of connected devices that make up your devices fleets. 
- Help you ensure that your IoT devices work properly and securely after they have been deployed. 
- Provides secure tunneling to access your devices, monitor their health, detect and remotely troubleshoot problems.
- Provides services to manage device software and firmware updates.

AWS IoT Device Defender:
- Helps you secure your fleet of IoT devices. 
- Continuously audits your IoT configurations to make sure that they aren’t deviating from security best practices. 
- Sends an alert when it detects any gaps in your IoT configuration that might create a security risk, such as identity certificates being shared across multiple devices or a device with a revoked identity certificate trying to connect to AWS IoT Core

    .
AWS IoT Things Graph:
- A service that lets you visually connect different devices and web services to build IoT applications.
- Provides a visual drag-and-drop interface for connecting and coordinating interactions between devices and web services, so that you can build IoT applications efficiently.

======================================

AWS IoT Analytics:
- Lets you efficiently run and operationalize sophisticated analytics on massive volumes unstructured IoT data. 
- Automates each difficult step that is required to analyze data from IoT devices. 
- Filters, transforms, and enriches IoT data before storing it in a time-series data store for analysis.
- You can analyze your data by running one-time or scheduled queries using the built-in SQL query engine or machine learning.

AWS IoT SiteWise:
- Collects, stores, organizes, and monitors data passed from industrial equipment by MQTT messages or APIs at scale by providing software that runs on a gateway in your facilities. 
- The gateway securely connects to your on-premises data servers and automates the process of collecting and organizing the data and sending it to the AWS Cloud.

AWS IoT Events:
- Detects and responds to events from IoT sensors and applications. 
- Events are patterns of data that identify more complicated circumstances than expected, such as motion detectors using movement signals to activate lights and security cameras. 
- AWS IoT Events continuously monitors data from multiple IoT sensors and applications, and integrates with other services, such as AWS IoT Core, IoT SiteWise, DynamoDB, and others to enable early detection and unique insights.
