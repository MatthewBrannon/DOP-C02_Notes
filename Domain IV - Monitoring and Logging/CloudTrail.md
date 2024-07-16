## AWS CloudTrail Cheat Sheet

- Actions taken by a user, role, or an AWS service in the [AWS Management Console](https://tutorialsdojo.com/aws-management-console/), AWS Command Line Interface, and AWS SDKs and APIs are recorded as **events**.
- CloudTrail is enabled on your AWS account when you create it.
- CloudTrail focuses on auditing API activity.
- View events in **Event History**, where you can view, search, and download the past 90 days of activity in your AWS account.
- CloudTrail Lake lets you aggregate, immutably store, and query your activity logs. You can enable this feature using AWS SDKs and CLI.
- CloudTrail Insights helps you identify unusual activity in your account such as spikes in resource provisioning or bursts of [AWS IAM](https://tutorialsdojo.com/aws-identity-and-access-management-iam/) actions.

## **Trails**

- - Create a **CloudTrail trail** to archive, analyze, and respond to changes in your AWS resources.
    - #### Types
        - A trail that applies to **all regions** – CloudTrail records events in each region and delivers the CloudTrail event log files to an S3 bucket that you specify. This is the default option when you create a trail in the CloudTrail console.
        - A trail that applies to **one region –** CloudTrail records the events in the region that you specify only. This is the default option when you create a trail using the AWS CLI or the CloudTrail API.
    - You can create an **organization trail** that will log all events for all AWS accounts in an organization created by AWS Organizations. Organization trails must be created in the management account.
    - By default, CloudTrail event log files are encrypted using Amazon S3 server-side encryption. You can also choose to encrypt your log files with an AWS Key Management Service key.
    - You can store your log files in your S3 bucket for as long as you want, and also define S3 lifecycle rules to archive or delete log files automatically.
    - If you want notifications about log file delivery and validation, you can set up Amazon SNS notifications.
    - CloudTrail publishes log files about every five minutes.

## **Events**

- - The record of an activity in an AWS account. This activity can be an action taken by a user, role, or service that is monitorable by CloudTrail.
    - #### Types
        - Management events
            - Logged by default
            - Management events provide insight into management operations performed on resources in your AWS account, also known as _control plane operations_.
        - Data events
            - Not logged by default
            - Data events provide insight into the resource operations performed on or in a resource, also known as _data plane operations_.
            - Data events are often high-volume activities.
        - Insights events
            - Not logged by default
            - Insights events capture unusual activity in your AWS account. If you have Insights events enabled, CloudTrail detects unusual activity and logs this to S3.
            - Insights events provide relevant information, such as the associated API, incident time, and statistics, that help you understand and act on unusual activity. 
            - Insights events are logged only when CloudTrail detects changes in your account’s API usage that differ significantly from the account’s typical usage patterns.
- For global services such as IAM, STS, CloudFront, and Route 53, events are delivered to any trail that includes global services, and are logged as occurring in US East (N. Virginia) Region.
- You can filter logs by specifying Time range and one of the following attributes: Event name, User name, Resource name, Event source, Event ID, and Resource type.

## **AWS CloudTrail Monitoring**

- - Use **CloudWatch Logs** to monitor log data. CloudTrail events that are sent to CloudWatch Logs can trigger alarms according to the metric filters you define.
    - To determine whether a log file was modified, deleted, or unchanged after CloudTrail delivered it, you can use CloudTrail log file integrity validation.

## **AWS CloudTrail Price**

- - The first copy of management events within each region is delivered free of charge. Additional copies of management events are charged.
    - Data events are recorded and charged only for the Lambda functions, DynamoDB tables, and S3 buckets you specify.
    - Once a CloudTrail trail is set up, S3 charges apply based on your usage, since CloudTrail delivers logs to an S3 bucket.

## **AWS CloudTrail Limits**

|   |   |   |
|---|---|---|
|**Resource**|**Default Limit**|**Comments**|
|Trails per region|5|A trail that applies to all regions counts as one trail in every region.<br><br>This limit cannot be increased.|
|Get, describe, and list APIs|10 transactions per second (TPS)|The maximum number of operation requests you can make per second without being throttled.<br><br>This limit cannot be increased.|
|LookupEvents API|2 transactions per second (TPS)|The maximum number of operation requests you can make per second without being throttled.<br><br>This limit cannot be increased.|
|All other APIs|1 transaction per second (TPS)|The maximum number of operation requests you can make per second without being throttled.<br><br>This limit cannot be increased.|
|Event data stores in CloudTrail Lake|5 per region|This limit cannot be increased.|
|Event selectors|5 per trail|This limit cannot be increased.|
|Advanced event selectors|500 conditions across all advanced event selectors|This limit cannot be increased.|
|Data resources in event selectors|250 across all event selectors in a trail|The total number of data resources cannot exceed 250 across all event selectors in a trail. The limit of number of resources on an individual event selector is configurable up to 250. This upper limit is allowed only if the total number of data resources does not exceed 250 across all event selectors.<br><br>This limit cannot be increased.|
