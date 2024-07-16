## AWS CloudFormation Cheat Sheet

- A service that gives developers and businesses an easy way to create a collection of related AWS resources and provision them in an orderly and predictable fashion.
## **Features**

- CloudFormation allows you to model your entire infrastructure in a text file called a **template**. You can use JSON or YAML to describe what AWS resources you want to create and configure. If you want to design visually, you can use _AWS CloudFormation Designer_.
- CloudFormation automates the provisioning and updating of your infrastructure in a safe and controlled manner. You can use **Rollback Triggers** to specify the [[CloudWatch]] alarm that CloudFormation should monitor during the stack creation and update process. If any of the alarms are breached, CloudFormation rolls back the entire stack operation to a previously deployed state.
- **CloudFormation Change Sets** allow you to preview how proposed changes to a stack might impact your running resources.
- **AWS StackSets** lets you provision a common set of AWS resources across multiple accounts and regions with a single CloudFormation template. StackSets takes care of automatically and safely provisioning, updating, or deleting stacks in multiple accounts and across multiple regions.
- CloudFormation enables you to build custom extensions to your stack template using AWS [[Lambda]].
- **CloudFormation registry** helps you discover and provision private and public extensions such as resources, modules, and hooks in your AWS CloudFormation templates.

## **CloudFormation vs [[Elastic Beanstalk]]**

- [[Elastic Beanstalk]] provides an **environment** to easily **deploy and run** applications in the cloud.
- CloudFormation is a convenient **provisioning mechanism** for a broad range of AWS resources.
## **Concepts**

- **Templates**
    - A JSON or YAML formatted text file.
    - CloudFormation uses these templates as blueprints for building your AWS resources.
- **Stacks**
    - Manage related resources as a single unit.
    - All the resources in a stack are defined by the stack’s CloudFormation template.
- **Change Sets**
    - Before updating your stack and making changes to your resources, you can generate a change set, which is a summary of your proposed changes.
    - Change sets allow you to see how your changes might impact your running resources, especially for critical resources, before implementing them.

![AWS Training AWS CloudFormation 2](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2018/12/AWS-Training-AWS-CloudFormation-2.jpg)

- With AWS CloudFormation and AWS [[CodePipeline]], you can use continuous delivery to automatically build and test changes to your CloudFormation templates before promoting them to production stacks.
- _CloudFormation artifacts_ can include a stack template file, a template configuration file, or both. AWS [[CodePipeline]] uses these artifacts to work with CloudFormation stacks and change sets.
    - **Stack Template File** – defines the resources that CloudFormation provisions and configures. You can use YAML or JSON-formatted templates.
    - **Template Configuration File** – a JSON-formatted text file that can specify template parameter values, a stack policy, and tags. Use these configuration files to specify parameter values or a stack policy for a stack.
- Through the AWS PrivateLink, you can use CloudFormation APIs inside of your Amazon VPC and route data between your VPC and CloudFormation entirely within the AWS network.

## **Stacks**

- If a resource cannot be created, CloudFormation rolls the stack back and automatically deletes any resources that were created. If a resource cannot be deleted, any remaining resources are retained until the stack can be successfully deleted.
- Stack update methods
    - Direct update
    - Creating and executing change sets
- When you update a stack, CloudFormation will only update resources that have been modified in the current stack template. And, while the update is being applied, resources that haven’t changed will continue to operate without any disruption.
- **Drift detection** enables you to detect whether a stack’s actual configuration differs, or has drifted, from its expected configuration. Use CloudFormation to detect drift on an entire stack, or on individual resources within the stack.
    - A resource is considered to have drifted if any if its actual property values differ from the expected property values.
    - A stack is considered to have drifted if one or more of its resources have drifted.
- To share information between stacks, export a stack’s output values. Other stacks that are in the same AWS account and region can import the exported values.
- You can nest stacks and create Microsoft Windows stacks.
- Using resource import, you can import or manage AWS resources that are created outside CloudFormation. You can also move resources between stacks by adding a _Retain_ deletion policy.
- Stack failure options allows you to troubleshoot resources in a _CREATE_FAILED_ or _UPDATE_FAILED_ status without rolling back successfully provisioned resources.

## **Templates**

- Templates include several major sections. The Resources section is the only required section.
- **CloudFormation Designer** is a graphic tool for creating, viewing, and modifying CloudFormation templates. You can diagram your template resources using a drag-and-drop interface, and then edit their details using the integrated JSON and YAML editor.
- Custom resources enable you to write custom provisioning logic in templates that CloudFormation runs anytime you create, update (if you changed the custom resource), or delete stacks.
- Template macros enable you to perform custom processing on templates, from simple actions like find-and-replace operations to extensive transformations of entire templates.
- Modules are building blocks that can be reused across different CloudFormation templates.
- You can use regular expressions when creating a template parameter.
- You can use CloudFormation to perform [[ECS]] blue/green deployments via AWS [[CodeDeploy]].

## **StackSets**

- CloudFormation StackSets allow you to roll out CloudFormation stacks over multiple AWS accounts and in multiple Regions with just a couple of clicks. StackSets is commonly used together with AWS Organizations to centrally deploy and manage services in different accounts.
- Administrator and target accounts – An _administrator account_ is the AWS account in which you create stack sets. A stack set is managed by signing in to the AWS administrator account in which it was created. A _target account_ is the account into which you create, update, or delete one or more stacks in your stack set.
- In addition to the organization’s management account, you can delegate other administrator accounts in your AWS Organization that can create and manage stack sets with service-managed permissions for the organization.
- Stack sets – A _stack set_ lets you create stacks in AWS accounts across regions by using a single CloudFormation template. All the resources included in each stack are defined by the stack set’s CloudFormation template. A stack set is a regional resource.
- Stack instances – A _stack instance_ is a reference to a stack in a target account within a region. A stack instance can exist without a stack; for example, if the stack could not be created for some reason, the stack instance shows the reason for stack creation failure. A stack instance can be associated with only one stack set.
- Stack set operations – Create stack set, update stack set, delete stacks, and delete stack set.
- Stack set operations options – Maximum concurrent accounts, failure tolerance, retain stacks, and region concurrency.
- Tags – You can add tags during stack set creation and update operations by specifying key and value pairs.
- For stack set operations and stack instances, StackSets generates status codes.
- You can configure an account gate or [[Lambda]] function to verify a target account meets certain requirements before it begins stack operations.
- You can also perform drift detection on a stack set to determine if any of the stack instances have drifted.
- Stack import operations:
    - Self-managed StackSets – Stacks can be imported into the administrator account or into other target accounts and AWS Regions.
    - Service-managed StackSets – Any stack in the same AWS Organizations as the management account can be imported.

## **AWS CloudFormation Monitoring**

- CloudFormation is integrated with AWS [[CloudTrail]], a service that provides a record of actions taken by a user, role, or an AWS service in CloudFormation. [[CloudTrail]] captures all API calls for CloudFormation as events, including calls from the CloudFormation console and from code calls to the CloudFormation APIs.

## **AWS CloudFormation Security**

- You can use IAM with CloudFormation to control what users can do with AWS CloudFormation, such as whether they can view stack templates, create stacks, or delete stacks.
- A _service role_ is an IAM role that allows CloudFormation to make calls to resources in a stack on your behalf. You can specify an IAM role that allows CloudFormation to create, update, or delete your stack resources.
- You can improve the security posture of your VPC by configuring CloudFormation to use an interface VPC endpoint.

## **AWS CloudFormation Pricing**

- No additional charge for CloudFormation. You pay for AWS resources created using CloudFormation in the same manner as if you created them manually.