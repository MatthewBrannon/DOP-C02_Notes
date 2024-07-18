## Amazon Elastic Container Registry

- A managed AWS Docker registry service.
- Amazon ECR is a regional service.

## **Features**

- ECR supports Docker Registry HTTP API V2 allowing you to use Docker CLI commands or your preferred Docker tools in maintaining your existing development workflow.
- ECR stores both the containers you create and any container software you buy through AWS Marketplace.
- ECR stores your container images in Amazon [[S3]].
- ECR supports the ability to define and organize repositories in your registry using namespaces.
- You can transfer your container images to and from Amazon ECR via HTTPS.

## **Components**

- **Registry**
    - A registry is provided to each AWS account; you can create image repositories in your registry and store images in them.
    - The URL for your default registry is https://_aws_account_id_.dkr.ecr._region_.amazonaws.com.
    - You must be authenticated before you can use your registry.
- **Authorization token**
    - Your Docker client needs to authenticate to ECR registries as an AWS user before it can push and pull images. The AWS CLI _get-login_ command provides you with authentication credentials to pass to Docker.
- **Repository**
    - An image repository contains your Docker images.
    - ECR uses resource-based permissions to let you specify who has access to a repository and what actions they can perform on it.
    - **ECR lifecycle policies** enable you to specify the lifecycle management of images in a repository.
- **Repository policy**
    - You can control access to your repositories and the images within them with repository policies.
- **Image**
    - You can push and pull Docker images to your repositories. You can use these images locally on your development system, or you can use them in [[ECS]] task definitions.
    - You can replicate images in your private repositories across AWS regions.

## **Amazon Elastic Container Registry Security**

- By default, [[IAM]] users don’t have permission to create or modify Amazon ECR resources or perform tasks using the Amazon ECR API.
- Use [[IAM]] policies to grant or deny permission to use ECR resources and operations.
- ECR partially supports resource-level permissions.
- ECR supports the use of customer master keys (CMK) managed by AWS [[KMS]] to encrypt container images stored in your ECR repositories.

## **Amazon Elastic Container Registry Pricing**

- You pay only for the amount of data you store in your repositories and data transferred to the Internet.