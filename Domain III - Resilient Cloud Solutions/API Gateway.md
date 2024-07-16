## Amazon API Gateway Cheat Sheet

- Enables developers to create, publish, maintain, monitor, and secure APIs at any scale.
- This is a HIPAA eligible service.
- Allows creating, deploying, and managing a RESTful API to expose backend HTTP endpoints, Lambda functions, or other AWS services.
- Together with Lambda, API Gateway forms the app-facing part of the AWS serverless infrastructure.
##  **Concepts**

- - **API deployment** – a point-in-time snapshot of your API Gateway API resources and methods. To be available for clients to use, the deployment must be associated with one or more API stages.
    - **API endpoints** – host names APIs in API Gateway, which are deployed to a specific region and of the format: _rest-api-id_.execute-api._region_.amazonaws.com
    - **API key** – An alphanumeric string that API Gateway uses to identify an app developer who uses your API.
    - **API stage** – A logical reference to a lifecycle state of your API. API stages are identified by API ID and stage name.
    - **Model** – Data schema specifying the data structure of a request or response payload.
    - **Private API** – An API that is exposed through interface VPC endpoints and isolated from the public internet
    - **Private integration** – An API Gateway integration type for a client to access resources inside a customer’s VPC through a private API endpoint without exposing the resources to the public internet.
    - **Proxy integration** – You can set up a proxy integration as an HTTP proxy integration type or a Lambda proxy integration type.
        - For the HTTP proxy integration, API Gateway passes the entire request and response between the frontend and an HTTP backend.
        - For the Lambda proxy integration, API Gateway sends the entire request as an input to a backend Lambda function.
    - **Usage plan** – Provides selected API clients with access to one or more deployed APIs. You can use a usage plan to configure throttling and quota limits, which are enforced on individual client API keys.

## **API Endpoint Types**

- - **Edge-optimized API endpoint**: The default host name of an API Gateway API that is deployed to the specified region while using a CloudFront distribution to facilitate client access typically from across AWS regions. API requests are routed to the nearest CloudFront Point of Presence.
    - **Regional API endpoint**: The host name of an API that is deployed to the specified region and intended to serve clients, such as EC2 instances, in the same AWS region. API requests are targeted directly to the region-specific API Gateway without going through any CloudFront distribution.
        - You can apply latency-based routing on regional endpoints to deploy an API to multiple regions using the same regional API endpoint configuration, set the same custom domain name for each deployed API, and configure latency-based DNS records in Route 53 to route client requests to the region that has the lowest latency.
    - **Private API endpoint**: Allows a client to securely access private API resources inside a VPC. Private APIs are isolated from the public Internet, and they can only be accessed using VPC endpoints for API Gateway that have been granted access.

## **Features**

- - API Gateway can execute Lambda code in your account, start Step Functions state machines, or make calls to Elastic Beanstalk, EC2, or web services outside of AWS with publicly accessible HTTP endpoints.
    - API Gateway helps you define plans that meter and restrict third-party developer access to your APIs.
    - API Gateway helps you manage traffic to your backend systems by allowing you to set throttling rules based on the number of requests per second for each HTTP method in your APIs.
    - You can set up a cache with customizable keys and time-to-live in seconds for your API data to avoid hitting your backend services for each request.
    - API Gateway lets you run multiple versions of the same API simultaneously with **API Lifecycle**.
    - After you build, test, and deploy your APIs, you can package them in an API Gateway usage plan and sell the plan as a Software as a Service (SaaS) product through AWS Marketplace.
    - API Gateway offers the ability to create, update, and delete documentation associated with each portion of your API, such as methods and resources.
    - Amazon API Gateway offers general availability of HTTP APIs, which gives you the ability to route requests to private ELBs AWS AppConfig, Amazon EventBridge, Amazon Kinesis Data Streams, [Amazon SQS](https://tutorialsdojo.com/amazon-sqs/), [AWS Step Functions](https://tutorialsdojo.com/aws-step-functions/) and IP-based services registered in AWS CloudMap such as ECS tasks. Previously, HTTP APIs enabled customers to only build APIs for their serverless applications or to proxy requests to HTTP endpoints.
    - You can create data mapping definitions from an HTTP API’s method request data (e.g. path parameters, query string, and headers) to the corresponding integration request parameters and from the integration response data (e.g. headers) to the HTTP API method response parameters.
    - Use wildcard custom domain names (*.example.com) to create multiple URLs that route to one API Gateway HTTP API.
    - You can configure your custom domain name to route requests to different APIs. Using multi-level base path mappings, you can implement path-based API versioning and migrate API traffic between APIs according to request paths with many segments.
- All of the APIs created expose **HTTPS endpoints only**. API Gateway does not support unencrypted (HTTP) endpoints.

## **Amazon API Gateway Monitoring**

- - API Gateway console is integrated with CloudWatch, so you get backend performance metrics such as API calls, latency, and error rates.
    - You can set up custom alarms on API Gateway APIs.
    - API Gateway can also log API execution errors to CloudWatch Logs.

## **Amazon API Gateway Security**

- - To authorize and verify API requests to AWS services, API Gateway can help you leverage signature version 4. Using signature version 4 authentication, you can use IAM and access policies to authorize access to your APIs and all your other AWS resources.
    - You can enable AWS WAF for your APIs in Amazon API Gateway, making it easier to protect your APIs against common web exploits such as SQL injection and Cross-Site Scripting (XSS).
    - For API Gateway HTTP APIs, in addition to the previously supported OIDC/OAuth2 authorization option, you can also secure them using Lambda authorizers and IAM authorizers.

## **Amazon API Gateway Pricing**

- - You pay only for the API calls you receive and the amount of data transferred out.
    - API Gateway also provides optional data caching charged at an hourly rate that varies based on the cache size you select.