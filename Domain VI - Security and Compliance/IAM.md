## AWS Identity and Access Management

- Control who is authenticated (signed in) and authorized (has permissions) to use resources.
- AWS account **root user** is a single sign-in identity that has complete access to all AWS services and resources in the account.

## **Features**

- You can grant other people permission to administer and use resources in your AWS account without having to share your password or access key.
- You can grant different permissions to different people for different resources.
- You can use IAM features to securely provide credentials for applications that run on [[EC2]] instances which provide permissions for your applications to access other AWS resources.
- You can add two-factor authentication to your account and to individual users for extra security.
- You can allow users to use **identity federation** to get temporary access to your AWS account.
- You receive AWS [[CloudTrail]] log records that include information about **IAM identities** who made requests for resources in your account.
- You use an **access key** (an access key ID and secret access key) to make programmatic requests to AWS. An Access Key ID and Secret Access Key can only be uniquely generated once and must be regenerated if lost.
- IAM has been validated as being compliant with Payment Card Industry (PCI) Data Security Standard (DSS).
- IAM is _eventually consistent_. IAM achieves high availability by replicating data across multiple servers within Amazon’s data centers around the world.
- IAM and AWS Security Token Service (STS) are offered at no additional charge.
- Your unique account sign-in page URL:  
    https://_My_AWS_Account_ID_.signin.aws.amazon.com/console/
- You can use IAM tags to add custom attributes to an IAM user or role using a tag key–value pair.
- You can generate and download a credential report that lists all users on your AWS account. The report also shows the status of passwords, access keys, and MFA devices.

## **Infrastructure Elements**

- #### **Principal**
    - An entity that can make a request for an action or operation on an AWS resource. Users, roles, federated users, and applications are all AWS principals.
    - Your AWS account root user is your _first principal_.
- #### **Request**
    - When a principal tries to use the [AWS Management Console](https://tutorialsdojo.com/aws-management-console/), the AWS API, or the AWS CLI, that principal sends a _request_ to AWS.
    - Requests includes the following information:
        - **Actions or operations** – the actions or operations that the principal wants to perform.
        - **Resources** – the AWS resource object upon which the actions or operations are performed.
        - **Principal** – the user, role, federated user, or application that sent the request. Information about the principal includes the policies that are associated with that principal.
        - **Environment data** – information about the IP address, user agent, SSL enabled status, or the time of day.
        - **Resource data** – data related to the resource that is being requested.
- #### **Authentication**
    - To authenticate from the console as a user, you must sign in with your user name and password.
    - To authenticate from the API or AWS CLI, you must provide your access key and secret key.
- #### **Authorization**
    - AWS uses values from the _request context_ to check for policies that apply to the request. It then uses the policies to determine whether to allow or deny the request.
    - Policies types can be categorized as _permissions policies_ or _permissions boundaries_
        - _Permissions policies_ define the permissions for the object to which they’re attached. These include identity-based policies, resource-based policies, and ACLs.
        - _Permissions boundary_ is an advanced feature that allows you to use policies to limit the maximum permissions that a principal can have.
    - To provide your users with permissions to access the AWS resources in their own account, you need **identity-based policies**.
    - **Resource-based policies** are for granting cross-account access.
    - Evaluation logic rules for policies:
        - By default, **all requests are denied**.
        - An _explicit allow_ in a permissions policy overrides this default.
        - A _permissions boundary_ overrides the allow. If there is a permissions boundary that applies, that boundary must allow the request. Otherwise, it is implicitly denied.
        - An explicit deny in any policy overrides any allows.
- #### **Actions or Operations**
    - Operations are defined by a service, and include things that you can do to a resource, such as viewing, creating, editing, and deleting that resource.
- #### **Resource**
    - An object that exists within a service. The service defines a set of actions that can be performed on each resource.

## **Users**

- **IAM Users**
    - Instead of sharing your root user credentials with others, you can create individual **IAM users** within your account that correspond to users in your organization. IAM users are not separate accounts; they are users within your account.
    - Each user can have its own password for access to the AWS Management Console. You can also create an individual access key for each user so that the user can make programmatic requests to work with resources in your account.
    - By default, a brand new IAM user has **NO permissions** to do anything.
    - Users are global entities.
- **Federated Users**
    - If the users in your organization already have a way to be authenticated, you can federate those user identities into AWS.
    - ![AWS Training IAM 2](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2018/12/AWS-Training-IAM-2.jpg)
- **IAM Groups**
    - An IAM group is a collection of IAM users.
    - You can organize IAM users into IAM groups and attach access control policies to a group.
    - A user can belong to multiple groups.
    - Groups cannot belong to other groups.
    - Groups do not have security credentials, and cannot access web services directly.
- **IAM Role**
    - A role does not have any credentials associated with it.
    - An IAM user can assume a role to temporarily take on different permissions for a specific task. A role can be assigned to a federated user who signs in by using an external identity provider instead of IAM.
    - **AWS service role** is a role that a service assumes to perform actions in your account on your behalf. This service role must include all the permissions required for the service to access the AWS resources that it needs.
        - **AWS service role for an [[EC2]] instance** is a special type of service role that a service assumes to launch an [[EC2]] instance that runs your application. This role is assigned to the [[EC2]] instance when it is launched.
        - **AWS service-linked role** is a unique type of service role that is linked directly to an AWS service. Service-linked roles are predefined by the service and include all the permissions that the service requires to call other AWS services on your behalf.
    - An instance profile is a container for an IAM role that you can use to pass role information to an [[EC2]] instance when the instance starts.
- Users or groups can have multiple policies attached to them that grant different permissions.

| **When to Create IAM User**                                                                                       | **When to Create an IAM Role**                                                                                                                       |
| ----------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| You created an AWS account and you’re the only person who works in your account.                                  | You’re creating an application that runs on an Amazon [[EC2]] instance and that application makes requests to AWS.                                   |
| Other people in your group need to work in your AWS account, and your group is using no other identity mechanism. | You’re creating an app that runs on a mobile phone and that makes requests to AWS.                                                                   |
| You want to use the command-line interface to work with AWS.                                                      | Users in your company are authenticated in your corporate network and want to be able to use AWS without having no sign in again (federate into AWS) |

## **Policies**

- Most permission policies are JSON policy documents.
- The IAM console includes _policy summary tables_ that describe the access level, resources, and conditions that are allowed or denied for each service in a policy.
- The _policy summary table_ includes a list of services. Choose a service there to see the _service summary_.
- This _summary table_ includes a list of the actions and associated permissions for the chosen service. You can choose an action from that table to view the action _summary_.
- To assign permissions to federated users, you can create an entity referred to as a **role** and define permissions for the **role**.
- **Identity-Based Policies**
- Permissions policies that you attach to a principal or identity.
    - **Managed policies** are standalone policies that you can attach to multiple users, groups, and roles in your AWS account.
    - **Inline policies** are policies that you create and manage and that are embedded directly into a single user, group, or role.
- **Resource-based Policies**
    - Permissions policies that you attach to a resource such as an Amazon [[S3]] bucket.
    - Resource-based policies are only inline policies.
    - **Trust policies** – resource-based policies that are attached to a role and define which principals can assume the role.

## **AWS Security Token Service (STS)**

- Create and provide trusted users with temporary security credentials that can control access to your AWS resources.
- Temporary security credentials are short-term and are not stored with the user but are generated dynamically and provided to the user when requested.
- By default, AWS STS is a global service with a single endpoint at https://sts.amazonaws.com.

## **Assume Role Options**

- AssumeRole – Returns a set of temporary security credentials that you can use to access AWS resources that you might not normally have access to. These temporary credentials consist of an access key ID, a secret access key, and a security token. Typically, you use _AssumeRole_ within your account or for cross-account access. 
    - You can include multi-factor authentication (MFA) information when you call _AssumeRole_. This is useful for cross-account scenarios to ensure that the user that assumes the role has been authenticated with an AWS MFA device.
- AssumeRoleWithSAML – Returns a set of temporary security credentials for users who have been authenticated via a SAML authentication response. This allows you to link your enterprise identity store or directory to role-based AWS access without user-specific credentials or configuration.
- AssumeRoleWithWebIdentity – Returns a set of temporary security credentials for users who have been authenticated in a mobile or web application with a web identity provider. Example providers include Amazon [[Cognito]], Login with Amazon, Facebook, Google, or any OpenID Connect-compatible identity provider.

## **STS Get Tokens**

- GetFederationToken – Returns a set of temporary security credentials (consisting of an access key ID, a secret access key, and a security token) for a federated user. You must call the GetFederationToken operation using the long-term security credentials of an IAM user. A typical use is in a proxy application that gets temporary security credentials on behalf of distributed applications inside a corporate network.
- GetSessionToken – Returns a set of temporary credentials for an AWS account or IAM user. The credentials consist of an access key ID, a secret access key, and a security token. You must call the GetSessionToken operation using the long-term security credentials of an IAM user. Typically, you use GetSessionToken if you want to use MFA to protect programmatic calls to specific AWS API operations.

## IAM Access Analyzer

- Provides policy checks that help you proactively validate policies when creating them. These checks analyze your policy and report errors, warnings, and suggestions with actionable recommendations that help you set secure and functional permissions.
- IAM Access Analyzer continuously monitors for new or updated resource policies and permissions granted for [[S3]] buckets, [[KMS]] keys, [[SQS]] queues, IAM roles, [[Lambda]] functions, and [[Secrets Manager]] secrets.

## IAM Identity Center

- The AWS IAM Identity Center is the successor to AWS Single Sign-On
    - Enable you to manage sign-in security for your workforce identities, also known as workforce users.
    - Manage your workforce’s sign-in security by creating or connecting your users and groups to AWS in one place.
    - Workforce identities
        - The members of your organization are also known as workforce users.
        - You can only have 1 identity source per AWS organization:
            - IAM Identity Center identity store
            - Active Directory
            - External identity provider
    - Multi-account permissions 
        - Enables you to assign workforce identities access to AWS accounts.
        - By enabling delegated administration, you can get the following benefits:
            - Reduces the number of people who need access to the management account, which helps to alleviate security concerns.
            - Allows select administrators to assign users and groups to applications as well as member accounts in your organization.
        - You can manage access to AWS resources across multiple AWS accounts using attribute-based access control (ABAC).
            - Requires fewer permission sets.
            - Permissions for new resources are automatically granted.
            - You can use employee attributes from the corporate directory.
            - Keep track of who is accessing your resources.
    - Application assignments
        - Assign users access to SaaS applications.
        - Supports the following types of applications:
            - Identity Center enabled applications
            - Cloud applications
            - Custom SAML 2.0 applications
    - You can also integrate AWS CLI version 2 with IAM Identity Center.

## **Best Practices**

- Lock Away Your AWS Account Root User Access Keys
- Create Individual IAM Users
- Use Groups to Assign Permissions to IAM Users
- Use AWS Defined Policies to Assign Permissions Whenever Possible
- Grant Least Privilege
- Use Access Levels to Review IAM Permissions
- Configure a Strong Password Policy for Your Users
- Enable MFA for Privileged Users
- Use Roles for Applications That Run on Amazon [[EC2]] Instances
- Use Roles to Delegate Permissions
- Do Not Share Access Keys
- Rotate Credentials Regularly
- Remove Unnecessary Credentials
- Use Policy Conditions for Extra Security
- Monitor Activity in Your AWS Account