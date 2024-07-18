## AWS CodePipeline
- A fully managed **continuous delivery service** that helps you automate your release pipelines for application and infrastructure updates.
- You can easily integrate AWS CodePipeline with third-party services such as GitHub or with your own custom plugin.

![AWS CodePipeline 1](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2019/05/AWS-CodePipeline-1.jpg)

## **Concepts**

- A **pipeline** defines your release process workflow, and describes how a new code change progresses through your release process.
    - A pipeline comprises a series of **stages** (e.g., build, test, and deploy), which act as logical divisions in your workflow. Each stage is made up of a sequence of actions, which are tasks such as building code or deploying to test environments.
        - Pipelines must have **at least two stages**. The first stage of a pipeline is required to be a source stage, and the pipeline is required to additionally have at least one other stage that is a build or deployment stage.
    - Define your pipeline structure through a **declarative JSON** document that specifies your release workflow and its stages and actions. These documents enable you to update existing pipelines as well as provide starting templates for creating new pipelines.
    - A **revision** is a change made to the source location defined for your pipeline. It can include source code, build output, configuration, or data. A pipeline can have multiple revisions flowing through it at the same time.
    - A **stage** is a group of one or more actions. A pipeline can have two or more stages.
    - An **action** is a task performed on a revision. Pipeline actions occur in a specified order, in serial or in parallel, as determined in the configuration of the stage.
        - You can add actions to your pipeline that are in an AWS Region different from your pipeline.
        - There are six types of actions
            - Source
            - Build
            - Test
            - Deploy
            - Approval
            - Invoke
    - When an action runs, it acts upon a file or set of files called **artifacts**. These artifacts can be worked upon by later actions in the pipeline. You have an artifact store which is an [[S3]] bucket in the same AWS Region as the pipeline to store items for all pipelines in that Region associated with your account.
    - The stages in a pipeline are connected by **transitions.** Transitions can be disabled or enabled between stages. If all transitions are enabled, the pipeline runs continuously.
    - An **approval action** prevents a pipeline from transitioning to the next action until permission is granted. This is useful when you are performing code reviews before code is deployed to the next stage.

![AWS CodePipeline 3](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2019/05/AWS-CodePipeline-3.jpg)

## **Features**

- AWS CodePipeline provides you with a graphical user interface to create, configure, and manage your pipeline and its various stages and actions.
    - A pipeline starts automatically (default) when a change is made in the source location, or when you manually start the pipeline. You can also set up a rule in [[CloudWatch]] to automatically start a pipeline when events you specify occur.
    - You can model your build, test, and deployment actions to run **in parallel** in order to increase your workflow speeds.
    - AWS CodePipeline can pull source code for your pipeline directly from AWS [[CodeCommit]], GitHub, Amazon [[ECR]], or Amazon [[S3]].
    - It can run builds and unit tests in AWS [[CodeBuild]].
    - It can deploy your changes using AWS [[CodeDeploy]], AWS [[Elastic Beanstalk]], Amazon [[ECS]], [AWS Fargate](https://tutorialsdojo.com/aws-fargate/), Amazon [[S3]], AWS Service Catalog, and/or AWS [[CloudFormation]].
    - You can use the CodePipeline Jenkins plugin to easily register your existing build servers as a custom action.
    - When you use the console to create or edit a pipeline that has a GitHub source, CodePipeline creates a **webhook**. A webhook is an HTTP notification that detects events in another tool, such as a GitHub repository, and connects those external events to a pipeline. CodePipeline deletes your webhook when you delete your pipeline.
- As a best practice, when you use a Jenkins build provider for your pipeline’s build or test action, install Jenkins on an Amazon [[EC2]] instance and configure a separate [[EC2]] instance profile. Make sure the instance profile grants Jenkins only the AWS permissions required to perform tasks for your project, such as retrieving files from Amazon [[S3]].
- AWS CodePipeline now supports **[Amazon VPC](https://tutorialsdojo.com/amazon-vpc/) endpoints powered by AWS PrivateLink**. This means you can connect directly to CodePipeline through a private endpoint in your VPC, keeping all traffic inside your VPC and the AWS network.
- You can view details for each of your pipelines, including when actions last ran in the pipeline, whether a transition between stages is enabled or disabled, whether any actions have failed, and other information. You can also view a history page that shows details for all pipeline executions for which history has been recorded. Execution history is retained for up to 12 months.

## **AWS CodePipelineLimits**

- Maximum number of total pipelines per Region in an AWS account is 300
    - Number of stages in a pipeline is minimum of 2, maximum of 10

## **AWS CodePipeline** **Pricing**

- You are charged per active pipeline each month. Newly created pipelines are free to use during the first 30 days after creation.