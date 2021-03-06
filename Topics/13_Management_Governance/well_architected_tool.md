# AWS Well-Architected Framework:
- Best practices developed by AWS Solutions Architects based on their years of experience building solutions across a wide variety of businesses.
- Provides a consistent approach for evaluating systems against the qualities that are expected from modern cloud-based systems.
- Based on the state of your architecture, the framework suggests improvements that you can make to better achieve those qualities. 
- Provides best practices for designing and operating reliable, secure, efficient, and cost-effective systems in the cloud.


# AWS Well-Architected Tool (AWS WA Tool):
- A service that provides a consistent process for measuring your architecture using AWS best practices. 
- Assists with documenting the decisions that you make.
- Provides recommendations for improving your workload based on best practices.
- Guides you in making your workloads more reliable, secure, efficient, and cost-effective.
- AWS customers use AWS WA Tool to document their architectures, provide product launch governance, and to understand and manage the risks in their technology portfolio. 
- End users can quickly deploy only the approved IT services they need, following the constraints set by your organization

# Regional and Global AWS Architecture

- There are 3 main type of architectures:
    - Small scale architectures: one region/one country
    - Small architecture with DR: one region + backup region for disaster recovery
    - Multiple region based systems
- Architectural components at global level:
    - Global Service Location and Discovery
    - Content Delivery (CDN) and optimization
    - Global health checks and Failover
- Regional components:
    - Regional entry point
    - Scaling and resilience
    - Application services and components
![Regional and Global Architecture](images/RegionalandGlobalInfrastructure2.png)