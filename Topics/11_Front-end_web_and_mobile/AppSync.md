# AWS AppSync:
- Provides a robust, scalable GraphQL interface for application developers to combine data from multiple sources, including Amazon DynamoDB, AWS Lambda, and HTTP REST APIs. 
- Integration with Amazon Cognito user pools for fine-grained access control at a per-field level.
- AppSync is a managed service which uses *GraphQL*
- It is used for building APIs on AWS which use *GraphQL*
- *GraphQL* makes it easy for applications to get the exact data they need. This includes combining data from multiple resources
- Datasets behind *GraphQL* can include:
    - NoSQL data stores
    - RDS databases
    - HTTP APIs
    - etc.
- AppSync integrates with (resolvers) DynamoDB, Aurora, ElasticSearch, etc.
- Supports customer resources using Lambda
- Provides support for real time data retrieval using WebSocket or MQTT on WebSocket protocols
- Mobile applications: replacement for Cognito Sync
- Requires a GraphQL schema for getting started
- Example for GraphQL schema:

    ```
    type Query {
        human(id: ID!): Human
    }

    type Human {
        name: String
        appearsIn: [Episode]
        starships: [Starship]
    }

    enum Episode {
        NEWHOPE
        EMPIRE
        JEDI
    }

    type Starship {
        name: String
    }
    ```

## Security

- Four ways we can authorize applications to interact with AppSync:
    - API_KEY
    - AWS_IAM
    - OPENID_CONNECT (OpenID Connect provider/ JWT)
    - AMAZON_COGNITO_USER_POOLS
- For custom domain & HTTPS, use CloudFront in front of AppSync