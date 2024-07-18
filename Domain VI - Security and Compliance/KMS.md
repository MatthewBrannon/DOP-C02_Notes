## **AWS KMS

- A managed service that enables you to easily encrypt your data. KMS provides a highly available key storage, management, and auditing solution for you to encrypt data within your own applications and control the encryption of stored data across AWS services.

## **Features**

- AWS KMS is integrated with [[CloudTrail]], which provides you the ability to audit who used which keys, on which resources, and when.
- AWS KMS keys are used to control access to data encryption keys that encrypt and decrypt your data.
- You can choose to have KMS automatically rotate KMS keys created within KMS once per year without the need to re-encrypt data that has already been encrypted with your KMS key.
- To help ensure that your keys and your data is highly available, KMS stores multiple copies of encrypted versions of your keys in systems that are designed for 99.999999999% durability.
- You can connect directly to AWS KMS through a private endpoint in your VPC instead of connecting over the Internet. When you use a VPC endpoint, communication between your VPC and AWS KMS is conducted entirely within the AWS network.
- You can define VPC Endpoint policies, enabling you to increase the granularity of your security controls by specifying which principals can access your endpoint, which API calls they can make, and which resources they can access.

## **Concepts**

- **AWS KMS keys** – You can use a KMS key to encrypt and decrypt up to 4 KB of data. Typically, you use KMS keys to generate, encrypt, and decrypt the data keys that you use outside of KMS to encrypt your data. Symmetric KMS keys are 256-bit advanced encryption standard (AES) keys that cannot be exported.
- There are three types of KMS keys:

| **Type of KMS key**   | **Can view** | **Can manage** | **Used only for my AWS account** |
| --------------------- | ------------ | -------------- | -------------------------------- |
| Customer managed keys | Yes          | Yes            | Yes                              |
| AWS managed keys      | Yes          | No             | Yes                              |
| AWS owned keys        | No           | No             | No                               |
- **_Customer managed keys_** are KMS keys that you create, own, and manage. You have full control over these KMS keys, including establishing and maintaining their key policies, [[IAM]] policies, and grants, enabling and disabling them, rotating their cryptographic material, adding tags, creating aliases that refer to the KMS keys, and scheduling the KMS keys for deletion.
    - **_AWS managed keys_** are KMS keys in your account that are created, managed, and used on your behalf by an AWS service that integrates with KMS. You can view the AWS managed keys in your account, view their key policies, and audit their use in [[CloudTrail]] logs. However, you cannot manage these AWS managed keys or change their permissions. And, you cannot use AWS managed keys in cryptographic operations directly; the service that creates them uses them on your behalf.
    - **_AWS owned keys_** are not in your AWS account. They are part of a collection of KMS keys that AWS owns and manages for use in multiple AWS accounts. AWS services can use AWS owned key to protect your data. You cannot view, manage, or use AWS owned keys, or audit their use.
- **Data keys** – Encryption keys that you can use to encrypt data, including large amounts of data and other data encryption keys.
    - AWS KMS can generate, encrypt, and decrypt data keys. However, KMS does not store, manage, or track your data keys, or perform cryptographic operations with data keys.
    - Data keys can be generated at 128-bit or 256-bit lengths and encrypted under a KMS key you define.
- **Envelope encryption** -The practice of encrypting plaintext data with a data key, and then encrypting the data key under another key. The top-level plaintext key encryption key is known as the _root key._
- **Encryption Context** – All KMS cryptographic operations accept an encryption context, an optional set of key–value pairs that can contain additional contextual information about the data.
- **Key Policies – When you create a KMS key, permissions that determine who can use and manage that KMS key are contained in a document called the _key policy_.**
- **Grants** – A grant is an alternative to the key policy. Grants are commonly used for temporary rights since they can be created, utilized, and deleted without affecting your key policies or [[IAM]] policies.
- **Grant Tokens** – When you create a grant, the permissions specified in the grant might not take effect immediately due to eventual consistency. If you need to mitigate the potential delay, use a grant token instead.
- When you enable **_automatic key rotation_** for a customer managed KMS key, AWS KMS generates new cryptographic material for the KMS key every year. KMS also saves the KMS Keys’ older cryptographic material so it can be used to decrypt data that it encrypted.
- An _alias_ is an optional display name for an AWS KMS Keys. Each KMS Key can have multiple aliases, but each alias points to only one KMS Key. The alias name must be unique in the AWS account and region.

## **Importing Keys**

- A KMS Key contains the **key material** used to encrypt and decrypt data. When you create a KMS key, by default AWS KMS generates the key material for that KMS key. But you can create a KMS key without key material and then import your own key material into that KMS key.
- When you import key material, you can specify an expiration date. When the key material expires, KMS deletes the key material and the KMS key becomes unusable. You can also delete key material on demand.

## **Deleting Keys**

- Deleting a KMS key deletes the key material and all metadata associated with the KMS key and is irreversible. You can no longer decrypt the data that was encrypted under that KMS key, which means that data becomes unrecoverable.
- You can create a [[CloudWatch]] alarm that sends you a notification when a user attempts to use the KMS key while it is pending deletion.
- You can temporarily disable keys so they cannot be used by anyone.
- KMS supports custom key stores backed by [AWS CloudHSM](https://tutorialsdojo.com/aws-cloudhsm/) clusters. A key store is a secure location for storing cryptographic keys.
- You can connect directly to AWS KMS through a private endpoint in your VPC instead of connecting over the internet. When you use a VPC endpoint, communication between your VPC and AWS KMS is conducted entirely within the AWS network.

## **Pricing**

- Each KMS key that you create in KMS, regardless of whether you use it with KMS-generated key material or key material imported by you, costs you until you delete it.
- For KMS keys with key material generated by KMS, if you opt-in to have the KMS keys automatically rotated each year, each newly rotated version will raise the cost of the KMS key per month.