## AWS Organizations

- It offers policy-based management for multiple AWS accounts.

## **Features**

- With Organizations, you can create groups of accounts and then apply policies to those groups.
- Organizations provides you a policy framework for multiple AWS accounts. You can apply policies to a group of accounts or all the accounts in your organization.
- AWS Organizations enables you to set up a single payment method for all the AWS accounts in your organization through **consolidated billing**. With consolidated billing, you can see a combined view of charges incurred by all your accounts, as well as take advantage of pricing benefits from aggregated usage, such as volume discounts for [[EC2]] and [[S3]].
- AWS Organizations, like many other AWS services, is **eventually consistent**. It achieves high availability by replicating data across multiple servers in AWS data centers within its region.

## **Administrative Actions in Organizations**

- Create an AWS account and add it to your organization, or add an existing AWS account to your organization.
- Organize your AWS accounts into groups called _organizational units_ (OUs).
- Organize your OUs into a hierarchy that reflects your company’s structure.
- Centrally manage and attach policies to the entire organization, OUs, or individual AWS accounts.

## **Concepts**

- An **organization** is a collection of AWS accounts that you can organize into a hierarchy and manage centrally.
- A **management** **account** is the AWS account you use to create your organization. You cannot change which account in your organization is the management account.
    - From the management account, you can create other accounts in your organization, invite and manage invitations for other accounts to join your organization, and remove accounts from your organization.
    - You can also attach policies to entities such as administrative roots, organizational units (OUs), or accounts within your organization.
    - The management account has the role of a payer account and is responsible for paying all charges accrued by the accounts in its organization.
- A **member account** is an AWS account, other than the management account, that is part of an organization. A member account can belong to only one organization at a time. The management account has the responsibilities of a payer account and is responsible for paying all charges that are accrued by the member accounts.
- An **administrative root** is the starting point for organizing your AWS accounts. The administrative root is the top-most container in your organization’s hierarchy. Under this root, you can create OUs to logically group your accounts and organize these OUs into a hierarchy that best matches your business needs.
- An **organizational unit** (OU) is a group of AWS accounts within an organization. An OU can also contain other OUs enabling you to create a hierarchy.
- A **policy** is a “document” with one or more statements that define the controls that you want to apply to a group of AWS accounts.
    - **Service control policy** (SCP) is a policy that specifies the services and actions that users and roles can use in the accounts that the SCP affects. SCPs are similar to [[IAM]] permission policies except that they don’t grant any permissions. Instead, SCPs are _filters_ that allow only the specified services and actions to be used in affected accounts.
    - ![AWS Training AWS Organizations 2](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2018/12/AWS-Training-AWS-Organizations-2.jpg)
- AWS Organizations has two available feature sets:
    - All organizations support **consolidated billing**, which provides basic management tools that you can use to centrally manage the accounts in your organization.
    - If you enable **all features**, you continue to get all the consolidated billing features plus a set of advanced features such as service control policies.
- You can remove an AWS account from an organization and make it into a standalone account.
- Organization Hierarchy
    - Including root and AWS accounts created in the lowest OUs, your hierarchy can be five levels deep.
    - Policies inherited through hierarchical connections in an organization.
    - Policies can be assigned at different points in the hierarchy.
- You can attach tags, or user-defined attributes, to Organizational Units, the organization’s root, and policies. These tags let you implement attribute-based access control (ABAC). ABAC is an authorization strategy that defines permissions based on tags attached to users and AWS resources.

## **AWS Organizations Pricing**

- This service is free.