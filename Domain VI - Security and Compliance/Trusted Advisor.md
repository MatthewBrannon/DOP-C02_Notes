## AWS Trusted Advisor Cheat Sheet

- Trusted Advisor analyzes your AWS environment and provides best practice recommendations in five categories:
    - Cost Optimization
    - Performance
    - Security
    - Fault Tolerance
    - Service Limits
- Access to the full set of Trusted Advisor checks is available to Business, Enterprise On-Ramp, and Enterprise Support plans.

![](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2018/12/AWS-Trusted-Advisor-1024x447.png)

## Concepts

- The AWS Support API gives you access to some of the AWS Support Center’s features and provides two different groups of operations:
    - **Support case management** – operations to manage your AWS support cases throughout their entire life cycle, from creation to resolution.
    - **Trusted Advisor** – operations to access AWS Trusted Advisor checks.
    - The endpoint to access the AWS Support API: _https://support.us-east-1.amazonaws.com_
- If you have a Business, Enterprise On-Ramp, or Enterprise Support plan, you can access all checks via the AWS Support API and the AWS CLI.
    - For the Basic and Developer Support plan, use the Trusted Advisor console to access core security checks and checks for service limits.
- You can use the Trusted Advisor console or the AWS Support API to perform operations on the following Trusted Advisor checks:
    - **Cost Optimization** – identify unused resources and opportunities to lower your bill.
    - **Performance** – improve the speed and responsiveness of your applications.
    - **Security** – recommends settings that can improve the security of your AWS solution.
    - **Fault** **Tolerance** – highlight redundancy shortfalls, current service limits, and overused resources.
    - **Service** **Limits** – shows the current usage limit for AWS services and resources.
- The summary checks are displayed on the Trusted Advisor dashboard.
    - **Action recommended (red)** – recommends an action for the check.
    - **Investigation recommended (yellow)** – detects a potential problem with the check.
    - **No problems detected (green)** – no issue identified for the check.
    - **Excluded items (gray)** – resources that you want a check to disregard.
- You can use the organizational view feature to generate a report for all AWS member accounts.
- You can also view your data in a dashboard and visualize your report information using the following services:
    - [Amazon S3](https://tutorialsdojo.com/amazon-s3/) – storage for resources.json report.
    - [AWS CloudFormation](https://tutorialsdojo.com/aws-cloudformation/) – a template that creates resources so that AWS services can access the S3 bucket’s report data.
    - [Amazon Athena](https://tutorialsdojo.com/amazon-athena/) – queries and analyzes the results of the report in the S3 bucket.
    - [Amazon QuickSight](https://tutorialsdojo.com/amazon-quicksight/) – a dashboard to view the results of the report.
- With **Trusted Advisor API operations**, you can write applications interacting with AWS Trusted Advisor.
    - Get the list of available Trusted Advisor checks
    - Refresh the list of available Trusted Advisor checks
    - Poll a Trusted Advisor to check for status changes
    - Request a Trusted Advisor check result
    - Print details of a Trusted Advisor check
- You can also use AWS Compute Optimizer to view the same recommendations in your Trusted Advisor checks.
- Your AWS account team can use **Trusted Advisor Priority** to proactively monitor your account and make prioritized recommendations when opportunities arise.
- Trusted Advisor Priority recommendations can come from either of two sources:
    - AWS services –  Trusted Advisor, AWS Security Hub, and AWS Well-Architected all generate recommendations automatically.
    - Your account team – create manual recommendations for risks found in your account.

## AWS Trusted Advisor Security

- You can use IAM policies to grant users or roles in your account access to AWS Trusted Advisor’s organizational view.
- With AWS Security Hub, you can view the Trusted Advisor check’s status, the list of affected resources, and then follow recommendations to address security issues.

## AWS Trusted Advisor Monitoring

- You can use Amazon EventBridge to detect when the status of your Trusted Advisor checks changes. Then, based on the rules you define, it performs one or more target actions whenever the status changes to a value specified in a rule.
- To create a rule for Trusted Advisor checks, you must have an AWS Support plan.
- You can also create alarms in Amazon CloudWatch to detect changes in the status of Trusted Advisor metrics.
- Supports logging a subset of the Trusted Advisor console actions and API operations as events in AWS CloudTrail.

## AWS Trusted Advisor Pricing

- By default, the Basic support plan is already included in your account.
- You only pay for the Developer, Business, Enterprise On-Ramp, and Enterprise Support plans