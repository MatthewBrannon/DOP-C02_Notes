## AWS Secrets Manager

- A secret management service that enables you to easily rotate, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle.

## **Features**

- AWS Secrets Manager encrypts secrets at rest using encryption keys that you own and store in AWS [[KMS]] customer managed keys. When you retrieve a secret, Secrets Manager decrypts the secret and transmits it securely over TLS to your local environment.
    - You can rotate secrets on a schedule or on demand by using the Secrets Manager console, AWS SDK, or AWS CLI.
    - Secrets Manager natively supports rotating credentials for databases hosted on Amazon [[RDS]] and [Amazon DocumentDB](https://tutorialsdojo.com/amazon-documentdb/) and clusters hosted on [Amazon Redshift](https://tutorialsdojo.com/amazon-redshift/)
    - You can extend Secrets Manager to rotate other secrets, such as credentials for Oracle databases hosted on [[EC2]] or OAuth refresh tokens, by using custom AWS [[Lambda]] functions.
- A secret consists of a set of credentials (user name and password), and the connection details used to access a secured service.
- A secret also contains **metadata** which include:
    - Basic information includes the name of the secret, a description, and the Amazon Resource Name (ARN) to serve as a unique identifier.
    - The ARN of the AWS [[KMS]] key Secrets Manager uses to encrypt and decrypt the protected text in the secret. If you don’t provide this information, Secrets Manager uses the default AWS [[KMS]] key for the account.
    - Information about how frequently to rotate the key and what [[Lambda]] function to use to perform the rotation.
    - A user-provided set of tags. You can attach tags as key-value pairs to AWS resources for organizing, logical grouping, and cost allocation.
- A secret can contain **versions**:
    - Although you typically only have one version of the secret active at a time, multiple versions can exist while you rotate a secret on the database or service. Whenever you change the secret, Secrets Manager creates a new version.
    - Each version holds a copy of the encrypted secret value.
    - Each version can have one or more _staging labels_ attached identifying the stage of the secret rotation cycle.
- Supported Secrets
    - Database credentials, on-premises resource credentials, SaaS application credentials, third-party API keys, and SSH keys. 
    - You can also store JSON documents.
- To retrieve secrets, you simply replace secrets in plain text in your applications with code to pull in those secrets programmatically using the Secrets Manager APIs.
- Secrets can be cached on the client side, and updated only during a secret rotation.
- During the secret rotation process, Secrets Manager tracks the older credentials, as well as the new credentials you want to start using, until the rotation completes. It tracks these different versions by using _staging labels_.

## **How Secret Rotation Works**

![AWS Secrets Manager](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/02/AWS-Secrets-Manager.jpg)

- The rotation function contacts the secured service authentication system and creates a new set of credentials to access the database. Secrets Manager stores these new credentials as the secret text in a new version of the secret with the AWSPENDING staging label attached.
    - The rotation function then tests the AWSPENDING version of the secret to ensure that the credentials work, and grants the required level of access to the secured service.
    - If the tests succeed, the rotation function then moves the label AWSCURRENT to the new version to mark it as the default version. Then, all of the clients start using this version of the secret instead of the old version. The function also assigns the label AWSPREVIOUS to the old version. The version that had AWSPREVIOUS staging label now has no label, and therefore deprecated.
- Network Setup for Secret Rotation
    - When rotating secrets on natively supported services, Secrets Manager uses [[CloudFormation]] to build the rotation function and configure the network connection between the two.
        - If your protected database service **runs in a VPC and is not publicly accessible**, then the [[CloudFormation]] template configures the **[[Lambda]] rotation function to run in the same VPC**. The rotation function can communicate with the protected service **directly within the VPC**.
        - If you run your protected service as a **publicly accessible resource**, in a VPC or not, then the [[CloudFormation]] template configures the **[[Lambda]] rotation function not to run in a VPC**. The [[Lambda]] rotation function communicates with the protected service **through the publicly accessible connection point**.
    - By default, the Secrets Manager endpoints run on the public Internet. If you run your [[Lambda]] rotation function and protected database or service in a VPC, then you must perform one of the following steps:
        - **Add a NAT gateway to your VPC**. This enables traffic that originates in your VPC to reach the public Secrets Manager endpoint. 
        - **Configure Secrets Manager service endpoints directly within your VPC**. This configures your VPC to intercept any request addressed to the public regional endpoint, and redirect the request to the private service endpoint running within your VPC.
- You can create two secrets that have different permissions
    - User Secret – can be used to connect to linked services, but it cannot be rotated. The user will have to wait for the master secret to be rotated and propagated for it to change.
    - Master Secret – has sufficient permissions to rotate secrets of linked services. This scenario is typically used when you have users that are actively using the old secret, and you do not want to break operations after you rotate the secret. You can have your users  update their clients first before using the newly rotated credentials.
- Secrets Manager lets you easily copy your secrets to multiple AWS Regions, which includes the primary secret and the associated metadata such as tags, resource policies and secret updates such as rotation.

## **AWS Secrets Manager Security**

- By default, Secrets Manager does not write or cache the secret to persistent storage.
    - By default, Secrets Manager only accepts requests from hosts that use the open standard Transport Layer Security (TLS) and Perfect Forward Secrecy.
    - You can control access to the secret using AWS [[IAM]] policies. 
    - You can tag secrets individually and apply tag-based access controls.
    - You can configure VPC endpoints to keep traffic between your VPC and Secrets Manager within the AWS network.
    - Secrets Manager does not immediately delete secrets. Instead, Secrets Manager immediately makes the secrets inaccessible and scheduled for deletion after a recovery window of a **minimum of seven days**. Until the recovery window ends, you can recover a secret you previously deleted. 
    - By using the CLI, you can delete a secret without a recovery window.

## **AWS Secrets Manager Compliance**

- Secrets Manager is HIPAA, PCI DSS and ISO, SOC, FedRAMP, DoD SRG, IRAP, and OSPAR compliant.

## **AWS Secrets Manager Pricing**

- You pay based on the number of secrets stored and API calls made per month.