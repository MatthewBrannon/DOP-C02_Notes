## **Amazon EventBridge Cheat Sheet**

**Amazon EventBridge** is a service that allows applications to communicate with each other using data from different sources in real time. It is a serverless event bus that acts as a centralized hub for ingesting events from various sources and directing them to the appropriate applications and services. This makes connecting applications easier and enables them to respond to data in real time.

## Concepts

- **Events:** An event in Amazon EventBridge represents a state change or occurrence of an incident within a system. Events have a JSON structure containing details like timestamps, event types, resource IDs, etc.
- **Event Patterns:** Event patterns allow users to filter events and match specific fields and values in the event payload. When creating an event pattern for an EventBridge rule, users specify the fields of an event that they want the pattern to match. The pattern should have the same JSON structure and field names as the incoming event.
- **Event Bus**: Acts as the event ingestion and delivery mechanism.
    
    ![Amazon EventBridge Cheat Sheet](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2024/03/Amazon-Eventbridge-Event-Bus-22Mar2024-jpg.webp)
    
    - - **Default Event Bus**: Each AWS account has a default event bus that receives events from AWS services like EC2, S3, etc.
        - **Custom Event Bus**: Users can create additional event buses called custom event buses to receive events from custom applications or third-party services.
            - **Configuring cross-account event buses** allows users to send and receive events from one AWS account to another using EventBridge.  
                To send and receive Amazon EventBridge events between AWS accounts:
                1. On the receiving account, edit the permissions on the target event bus to allow the sending account to send events to it. This is done by attaching an IAM policy to the event bus that grants the sending account permissions.
                2. On the sending account, create a rule with the target as the event bus in the receiving account. If the sending account inherits permissions from an AWS Organization, it also needs an IAM role to send events to the receiving account.
                3. On the receiving account, create rules to match and process the events received from the sending account.
- **Event Rule**: An EventBridge rule receives incoming events and sends them to targets for processing.
    - - **Event Pattern Rule**: These rules match events based on patterns in the event payload. For example, matching EC2 instance state change events where the state is “running”. The pattern can include filters for event metadata, data, and content.
        - **Schedule Rule**: These rules trigger targets on a scheduled frequency, such as every hour. This allows for the periodic triggering of lambdas or other targets.
- **Global Endpoints**: Enables users to route events to EventBridge in any region, improving the resilience of applications.
    
- **Archives and Replays**: Archives allow users to store events for a specified period. Replays enable users to resend events from an archive to the same or different targets, which is useful for testing or recovering from failures.
    
- **Security:** To use EventBridge, AWS needs credentials to control what resources, actions, and services can be accessed on users’ behalf. EventBridge uses both identity-based policies and resource-based policies to control access. Identity-based policies are attached to IAM identities (users or roles), while resource-based policies are attached to the EventBridge resource itself.
    
    For example, users may create a rule that triggers a Lambda function when an event occurs. The rule would need permission to invoke the Lambda function on users’ behalf. Users can assign an IAM role with `lambda:InvokeFunction` permission to the rule. When the event triggers the rule, EventBridge will be allowed to invoke the Lambda function using that role.
    
    Similarly, if a rule targets an SNS topic, the role assigned to the rule needs `sns:Publish` permission to the relevant SNS topic ARN. This allows EventBridge to publish the topic on your behalf when the rule is triggered.
    

## Features

### **1. EventBridge Pipes**

**EventBridge Pipes** allows users to connect event producers (sources) with consumers (targets) through a point-and-click interface, eliminating the need for custom code to filter and transform events.

![Amazon EventBridge Cheat Sheet](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2024/03/Amazon-EventBridge-Cheat-Sheet-Pipe-Diagram-Mar-08-24-jpg.webp)

**Pipe:** A pipe is a conduit that routes events from a single source to a single target. It can be configured to filter specific events and enrich the event data before it reaches the target.

**Source:** In EventBridge, pipes receive event data from various sources. If the source enforces event order, the pipes will maintain that order all the way to the target destination.

The following are some examples of event sources:

- Amazon SQS
- Amazon Kinesis
- Amazon DynamoDB
- Amazon Managed Streaming Kafka
- Self-managed Kafka
- Amazon MQ
- Custom Applications
- Third-party Software

A pipe routes events via point-point integration between a single source and a single target. The pipe allows users to:

1. **Filter:** Filtering in EventBridge pipes allows you to process only a subset of events from the source that matches certain criteria. To configure filtering on a pipe, users define an event pattern the pipe uses to determine which events to send to the target.
2. **Enrichment:** Users can enhance the data from the source before sending it to the target. For example, users might receive ‘Ticket created’ events that don’t include the full ticket data.

**Target:** After filtering and enhancing the event data, users can designate the pipeline and direct it towards a particular destination, such as an Amazon CloudWatch log group or an Amazon Kinesis stream.

Common targets used are:

- Amazon SQS
- AWS Step Functions
- Amazon Kinesis Data Streams
- Amazon Kinesis Data Firehose
- Amazon SNS
- Amazon ECS
- Event buses
- API Destination

Some common use cases of EventBridge Pipes include:

- Streaming data from services like DynamoDB or SQS to analytics services like Kinesis for real-time processing.
- Streaming application logs from CloudWatch Logs to a target like Kinesis Data Streams for downstream processing. This allows aggregating logs from multiple sources in one central place.
- Copying data from a source like DynamoDB streams to a target like S3 for archival and analysis. The pipe can filter and select only the required data to stream.

### **2. EventBridge Scheduler and Scheduled-based EventBridge rules**

**EventBridge Scheduler** is a serverless scheduling service that allows for the creation, execution, and management of scheduled tasks on a massive scale. It supports invoking various AWS services as targets, providing a centralized management console for all users’ scheduled jobs.  
![Amazon EventBridge Cheat Sheet](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2024/03/Amazon-EventBridge-Cheat-Sheet-Schedule-Diagram-Mar-08-24-jpg.webp)

**Scheduled-based EventBridge rules** allow users to schedule rules to run on a specific date or time. With these rules, users can define the schedule using either a cron expression or a rate expression.

EventBridge Scheduler has a significant improvement over scheduled-based EventBridge rules. While both can trigger targets on time schedules, EventBridge Scheduler offers several advantages:

1. EventBridge Scheduler allows for the creation of schedules independently without needing to create an event bus or rule, which is simpler than using scheduled rules.
2. EventBridge Scheduler supports features like time zones, increased scale to handle thousands of schedules, custom payloads for schedule targets, advanced time expressions, and a dashboard to monitor schedules.
3. With EventBridge Scheduler, it is easier to manage related schedules as a group with options for access control and execution roles.

### 3. Schema Registry

A **schema registry** stores JSON schemas that define the structure and content of events. It allows event producers and consumers to have a common understanding of the event format.

**Schema discovery** is a feature that automatically detects schemas from events and adds them to the discovered schema registry. It also tracks changes to schemas over time. This helps generate strongly typed code bindings to represent events as objects.

![Amazon-EventBridge-Event-Bus-Diagram](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2024/03/Amazon-EventBridge-Event-Bus-Diagram-Apr-01-2024.png)

Amazon EventBridge maintains two default schema registries:

1. **AWS event schema registry** – Contains schemas for events generated by AWS services. These are predefined.
2. **Discovered schema registry** – Contains schemas discovered by enabling schema discovery on an EventBridge event bus. This automatically extracts schemas from events sent to the bus.

Users can also create custom registries to organize schemas of their applications.

## Additional Information

Here are some additional important things to know about Amazon EventBridge:

1. **Event structure** – Events should follow a JSON schema standard for validation and ease of consumption by targets. Context, metadata, and timestamps are useful to include.
2. **Event sources** – Supported sources include API calls, database changes, code commits from various AWS services and third-party software.
3. **Event routing** – Events can be routed using event buses for many-to-many routing or event pipes for one-to-one routing.
5. **Debugging** – Tools are available to monitor for errors and latency issues and debug event delivery problems.
6. **API Destinations** – This feature allows for the secure integration of SaaS applications by sending events back to on-premises or third-party systems.

## **Use Cases**

1. **Application integration** – Use EventBridge to integrate applications together by routing events between them in real-time. This allows for building event-driven architectures.
2. **Serverless application development** – Build serverless applications using EventBridge to connect event sources like APIs to event-driven compute on AWS Lambda.
3. **Automation and workflow** – Automate tasks and build workflows using EventBridge rules to trigger actions in response to events. For example, starting an EC2 instance when a file is uploaded to S3.
4. **Data processing** – Process streaming data by routing events from sources like Kinesis Data Streams to targets like Kinesis Data Analytics for real-time analytics.
5. **Application monitoring** – Monitor applications by routing application events from sources like CloudWatch Logs to targets like Step Functions for orchestrating alerts and remediation workflows.
6. **Third-party integration** – Integrate SaaS applications and on-premises systems with AWS using the EventBridge API Destinations feature to exchange events bi-directionally in a secure manner.

## **Best Practices**

Here are some best practices when using Amazon EventBridge:
1. Enable event replication across multiple AWS regions for high availability and disaster recovery. EventBridge supports replicating events from one region to another.
2. Configuring adequate rules and targets to handle users’ expected event throughput can prevent event throttling. Monitoring throttling metrics can also help users add more capacity if needed.
3. Use subscriber health metrics in Amazon Route 53 for event-driven applications. This allows traffic to be routed away from unhealthy targets before they start dropping events.
4. Structure event data following JSON schema standards for validation and ease of consumption. Provide context, metadata, and timestamps with events.
5. Implement filtering rules to route only relevant events to appropriate targets. This avoids overloading targets and improves efficiency.
6. Monitor event delivery latency and errors. If latency is high or errors occur frequently, troubleshoot and optimize patterns, transformations, and targets.
7. Use dead-letter queues for failed events and have retry policies configured to process them again automatically.

## Pricing

Amazon EventBridge pricing is primarily based on the number of events published to the service, the number of schema registry API calls, and the use of features like EventBridge Pipes and Scheduler. The pricing model includes:

- **Events**: Charged per million events ingested into EventBridge.
- **Schema Discovery and Registry**: Charged per million API calls made for schema discovery and registry.
- **EventBridge Pipes**: Priced based on the amount of data processed.
- **EventBridge Scheduler**: Costs are incurred based on the number of scheduled invocations and their associated resources.

Pricing can vary by region and specific usage, so it’s important to consult the AWS pricing page for the most up-to-date information.