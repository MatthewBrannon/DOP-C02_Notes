## AWS Cloud Development Kit Cheat Sheet

- The AWS CDK is an open-source software development framework for defining cloud infrastructure in code and provisioning it through AWS [[CloudFormation]].
- It provides a high-level object-oriented abstraction on top of AWS [[CloudFormation]] that allows you to use pre-built cloud components called _constructs_.
- It uses familiar programming languages such as _TypeScript_, _JavaScript_, _Python_, _C#_, _.Net_, _Java_, and _Go_ and deploys everything as [[CloudFormation]] stacks. 

## **Concepts:**

- Stacks 
    - a deployment of AWS resources defined in your CDK app. Each stack is a [[CloudFormation]] stack and can contain multiple AWS resources. 
- Constructs: 
    - pre-built cloud components that can be used to model and provision resources in your CDK app. They are the building blocks of your CDK infrastructure.
    - CDK constructs come in three levels of abstraction:
        - **L1 constructs (also called CFN resources)** are low-level constructs that map directly to components that are available in [[CloudFormation]]. For example, a security group, [[S3]] bucket, or a DynamoDB table.
- - - **L2 constructs** provide the same functionality as [[CloudFormation]] resources but with added convenience by including the default settings, repetitive code, and connecting logic that you would otherwise have to write with a native [[CloudFormation]] resource.
- - - **L3 constructs (also called patterns)** define infrastructure that follows best practices and conventions, which can help you improve the structure and maintainability of your CDK apps. For example, you can easily build an API Gateway API backed by a [[Lambda]] function using the `_aws-apigateway.LambdaRestApi_` construct.
- App
    - A CDK app is a container for one or more stacks. It defines the cloud resources that will be created and how they will be connected. 
- Environment 
    - A logical deployment target for a stack. Environments can be used to deploy to different accounts, regions, or stages (e.g., dev, staging, prod) 
- `cdk.out` directory: 
    - This directory contains the synthesizer’s output, including the [[CloudFormation]] template and asset files. cdk.context.json: This file contains context data that can be passed to the CDK app at runtime, such as environment variables and stack-specific settings. 
- Stack drift
    - a feature that allows you to detect and compare the actual stack resources to the expected stack resources in your CloudFormation template.

## **Advantages of using AWS Cloud Development Kit (CDK)**

- Higher-level abstraction
    - allows developers to express their infrastructure more naturally and intuitively using their preferred programming languages. Constructs can help you create complex infrastructure with minimal code.
- Improved collaboration and testing
    - The CDK allows you to model and provision your cloud resources using Infrastructure as Code (IAC), which enables versioning, testing, and collaboration on infrastructure. 
- Easier CI/CD integration
    - The CDK can be integrated with other AWS services, such as [[CodePipeline]], [[CodeBuild]], and [[CodeDeploy]] for CI/CD. This makes it easier to automate the deployment and management of your infrastructure.

## **AWS CDK Pricing**

- AWS CDK comes at no extra cost, you will be billed for the AWS resources (e.g., [[EC2]] instances, EBS volumes, Application Load Balancers) created by CDK in the same way as if you had created them manually.