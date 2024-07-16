## Amazon Cognito Cheat Sheet

- A user management and authentication service that can be integrated to your **web or mobile applications**. Amazon Cognito also enables you to authenticate users through an **external identity provider** and provides **temporary security credential**s to access your app’s backend resources in AWS or any service behind Amazon API Gateway. Amazon Cognito works with external identity providers that support SAML or OpenID Connect, social identity providers (Facebook, Twitter, Amazon, Google, Apple) and you can also integrate your own identity provider.
- An Amazon Cognito ID token is represented as a **JSON Web Token** (JWT). Amazon Cognito uses JSON Web Tokens for token authentication.

## **How It Works**

![Amazon Cognito](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/02/Amazon-Cognito.jpg)

## **User Pools**

- - User pools are user directories that provide sign-up and sign-in options for your app users.
    - Users can sign in to your web or mobile app through Amazon Cognito, or federate through a third-party identity provider (IdP).
    - You can use the aliasing feature to enable your users to sign up or sign in with an email address and a password or a phone number and a password.
    - User pools are each created in one AWS Region, and they store the user profile data only in that region. You can also send user data to a different AWS Region.
    - Tokens provided through user pools:
        - Access tokens contain scopes and groups and are used to grant access to authorized resources. Access tokens can be configured to expire in as little as five minutes or as long as 24 hours.
        - Refresh tokens contain the information necessary to obtain a new ID or access token. Refresh tokens can be configured to expire in as little as one hour or as long as ten years.
    - A User Pool is like a _directory_ of users.
    - Manage Users
        - After you create a user pool, you can create, confirm, and manage users accounts. 
        - Amazon Cognito User Pools groups lets you manage your users and their access to resources by mapping IAM roles to groups.
        - User accounts are added to your user pool in one of the following ways:
        - The user signs up in your user pool’s client app, which can be a mobile or web app.
        - You can import the user’s account into your user pool.
        - You can create the user’s account in your user pool and invite the user to sign in.
        - Sign up authflow below

![Amazon Cognito](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/02/Amazon-Cognito2.jpg)

## **Identity Pools** 

- - Use this feature if you want to federate users to your AWS services.
    - Identity pools enable you to grant your users temporary AWS credentials to access AWS services, such as [Amazon S3](https://tutorialsdojo.com/amazon-s3/) and [DynamoDB](https://tutorialsdojo.com/amazon-cognito/DynamoDB).
    - Identity pools support anonymous guest users, as well as the following identity providers:
        - - Amazon Cognito user pools
            - Social sign-in with Facebook, Google, and Login with Amazon
            - OpenID Connect (OIDC) providers
            - SAML identity providers
            - Developer authenticated identities
    - To save user profile information, your identity pool needs to be integrated with a user pool.
    - Amazon Cognito Identity Pools can support unauthenticated identities by providing a unique identifier and AWS credentials for users who do not authenticate with an identity provider.
    - The permissions for each authenticated and non-authenticated user are controlled through IAM roles that you create.
    - Once you have an OpenID Connect token, you can then trade this for temporary AWS credentials via the _AssumeRoleWithWebIdentity_ API call in AWS Security Token Service (STS). This call is no different than if you were using Facebook, Google+, or Login with Amazon directly, except that you are passing an Amazon Cognito token instead of a token from one of the other public providers.

## **Common Use Cases**

- - Enable your users to authenticate with a user pool.

![Amazon Cognito](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/02/Amazon-Cognito3.jpg)

- - After a successful user pool sign-in, your web or mobile app will receive user pool tokens from Amazon Cognito. You can use those tokens to control access to your server-side resources.

![Amazon Cognito](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/02/Amazon-Cognito4.jpg)

- - Access resources with API Gateway and Lambda with a User Pool. API Gateway validates the tokens from a successful user pool authentication, and uses them to grant your users access to resources including Lambda functions, or your own API.  
        

![Amazon Cognito](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/02/Amazon-Cognito5.jpg)

- - After a successful user pool authentication, your app will receive user pool tokens from Amazon Cognito. You can exchange them for temporary access to other AWS services with an identity pool.  
        

![Amazon Cognito](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/02/Amazon-Cognito7.jpg)

- - Enable your users access to AWS services through an identity pool. In exchange, the identity pool grants temporary AWS credentials that you can use to access other AWS services.  
        

![Amazon Cognito](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/02/Amazon-Cognito8.jpg)

- - Grant your users access to AWS AppSync resources with tokens from a successful Amazon Cognito authentication (from a user pool or an identity pool).
    - Amazon Cognito is also commonly used together with AWS Amplify, a framework for developing web and mobile applications with AWS services.
- Amazon Cognito Sync
    - Store and sync data across devices using Cognito Sync.
    - You can programmatically trigger the sync of data sets between client devices and the Amazon Cognito sync store by using the synchronize() method in the AWS Mobile SDK. The synchronize() method reads the **latest version** of the data available in the Amazon Cognito sync store and compares it to the local, cached copy. After comparison, the synchronize() method writes the latest updates as necessary to the local data store and the Amazon Cognito sync store.
    - The Amazon Cognito Sync store is a key/value pair store linked to an Amazon Cognito identity. There is no limit to the number of identities you can create in your identity pools and sync store.
    - Each user information store can have a maximum size of 20MB. Each data set within the user information store can contain up to 1MB of data. Within a data set you can have up to 1024 keys.
    - With Cognito Streams, you can push sync store data to a Kinesis stream in your AWS account. 

- Advanced Security Features
    - When Amazon Cognito detects unusual sign-in activity, such as sign-in attempts from new locations and devices, it assigns a risk score to the activity and lets you choose to either prompt users for additional verification or block the sign-in request.
    - Users can verify their identities using SMS or a Time-based One-time Password (TOTP) generator.
    - When Amazon Cognito detects users have entered credentials that have been compromised elsewhere, it prompts a password change.

- Integration with AWS Lambda
    - You can create an [AWS Lambda](https://tutorialsdojo.com/aws-lambda/) function and then trigger that function during user pool operations such as user sign-up, confirmation, and sign-in (authentication) with a Lambda trigger.
    - Amazon Cognito invokes Lambda functions synchronously. When called, your Lambda function must respond within 5 seconds. If it does not, Amazon Cognito retries the call. After 3 unsuccessful attempts, the function times out.
    - You can create a Lambda function as a backend to Cognito that serves auth challenges to users signing in.

## **Amazon Cognito Pricing**

- - If you are using Cognito Identity to create a User Pool, you pay based on your monthly active users (MAUs) only. A user is counted as a MAU if, within a calendar month, there is an identity operation related to that user, such as sign-up, sign-in, token refresh or password change.
        - The Cognito Your User Pool feature has a free tier of 50,000 MAUs for users who sign in directly to Cognito User Pools or through social identity providers, and 50 MAUs for users federated through SAML 2.0 based identity providers.
    - You pay an additional fee when you enable advanced security features for Amazon Cognito.
    - Amazon Cognito uses Amazon SNS for sending SMS messages for Multi-Factor Authentication (MFA) and phone number verification, so there are associated SNS costs as well.