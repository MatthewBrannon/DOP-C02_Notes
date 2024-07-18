## AWS Fargate

- A serverless compute engine for containers that works with both Amazon [[ECS]] and Amazon [[EKS]].
- With Fargate, no manual provisioning, patching, cluster capacity management, or any infrastructure management required.

## **How It Works**

![AWS Fargate](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/02/AWS-Fargate.jpg)

## **Use Case**

- Launching containers without having to provision or manage [[EC2]] instances.
    - If you want a managed service for container cluster management.
- Configurations
    - Amazon [[ECS]] task definitions for Fargate require that you specify CPU and memory at the task level (task definition).
    - Amazon [[ECS]] task definitions for Fargate support the ulimits parameter to define the resource limits to set for a container.
    - Amazon [[ECS]] task definitions for Fargate support the awslogs, splunk, firelens, and fluentd log drivers for the log configuration.
    - When provisioned, each Fargate task receives the following storage:
        - 10 GB of Docker layer storage
        - An additional 4 GB for volume mounts.
    - Task storage is ephemeral.
    - If you have a service with running tasks and want to update its platform version, you can update your service, specify a new platform version, and choose Force new deployment. Your tasks are redeployed with the **latest** platform version.
    - If your service is scaled up without updating the platform version, those tasks receive the platform version that was specified on the service’s current deployment.
    - **Amazon [[ECS]] Exec** is a way for customers to execute commands in a container running on Amazon [[EC2]] instances or AWS Fargate. [[ECS]] Exec gives you interactive shell or single command access to a running container.

## **AWS Fargate Network**

- Amazon [[ECS]] task definitions for Fargate require that the network mode is set to awsvpc. The awsvpc network mode provides each task with its own elastic network interface.

## **AWS Fargate Compliance**

- PCI DSS Level 1, ISO 9001, ISO 27001, ISO 27017, ISO 27018, SOC 1, SOC 2, SOC 3, and HIPAA

## **AWS Fargate Pricing**

- You pay for the amount of vCPU and memory resources consumed by your containerized applications.