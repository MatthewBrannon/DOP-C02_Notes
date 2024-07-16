## AWS CodeBuild Cheat Sheet

- A fully managed **continuous integration service** that compiles source code, runs tests, and produces software packages that are ready to deploy.

## **Concepts**

- - A **build project** defines how [[CodeBuild]] will run a build. It includes information such as where to get the source code, which build environment to use, the build commands to run, and where to store the build output.
    - A **build environment** is the combination of operating system, programming language runtime, and tools used by [[CodeBuild]] to run a build.
    - The **build specification** is a YAML file that lets you choose the commands to run at each phase of the build and other settings. Without a build spec, [[CodeBuild]] cannot successfully convert your build input into build output or locate the build output artifact in the build environment to upload to your output bucket.
        - If you include a build spec as part of the source code, by default, the build spec file must be named buildspec.yml and placed in the root of your source directory.
    - A collection of input files is called **build input artifacts or build input** and a deployable version of a source code is called **build output artifact or build output**.

## **Features**

- - AWS CodeBuild runs your builds in preconfigured build environments that contain the operating system, programming language runtime, and build tools (such as Apache Maven, Gradle, npm) required to complete the task. You just specify your source code’s location and select settings for your build, such as the build environment to use and the build commands to run during a build.
    - AWS CodeBuild builds your code and stores the artifacts into an [[S3]] bucket, or you can use a build command to upload them to an artifact repository.
    - AWS CodeBuild provides build environments for
        - Java
        - Python
        - Node.js
        - Ruby
        - Go
        - Android
        - .NET Core for Linux
        - Docker
    - You can define the specific commands that you want AWS [[CodeBuild]] to perform, such as installing build tool packages, running unit tests, and packaging your code.
    - You can choose from **three levels of compute capacity** that vary by the amount of CPU and memory to best suit your development needs.
        - Build.general1.small – 3GB memory, 2 vCPU
        - Build.general1.medium – 7GB memory, 4 vCPU
        - Build.general1.large – 15GB memory, 8 vCPU
    - You can integrate [[CodeBuild]] into existing CI/CD workflows using its source integrations, build commands, or Jenkins integration.
    - [[CodeBuild]] can connect to AWS [[CodeCommit]], [[S3]], GitHub, GitHub Enterprise, and Bitbucket to pull source code for builds.
    - [[CodeBuild]] allows you to use Docker images stored in another AWS account as your build environment, by granting resource level permissions.
    - It now allows you to access Docker images from any private registry as the build environment. Previously, you could only use Docker images from public DockerHub or Amazon [[ECR]] in CodeBuild.
    - You can access your past build results through the console, [[CloudWatch]], or the API. The results include outcome (success or failure), build duration, output artifact location, and log location.
    - You can automate your release process using **AWS [[CodePipeline]] to test your code and run your builds with CodeBuild.
- Steps in a Build Process
    - CodeBuild will create a temporary compute container of the class defined in the build project
    - CodeBuild loads it with the specified runtime environment
    - CodeBuild downloads the source code
    - CodeBuild executes the commands configured in the project
    - CodeBuild uploads the generated artifact to an [[S3]] bucket
    - Then it destroys the compute container
- Build Duration is calculated in minutes, from the time you submit your build until your build is terminated, rounded up to the nearest minute.
- You can save time when your project builds by using a cache. A build project can use one of two types of caching:
    - Amazon [[S3]] – stores the cache in an Amazon [[S3]] bucket that is available across multiple build hosts. This is a good option for small intermediate build artifacts that are more expensive to build than to download. Not the best option for large build artifacts because they can take a long time to transfer over your network, which can affect build performance.
    - Local – stores a cache locally on a build host that is available to that build host only. This is a good option for large intermediate build artifacts because the cache is immediately available on the build host. Build performance is not impacted by network transfer time.
    - If you use a local cache, you must choose one or more of three cache modes:
        - source cache
        - Docker layer cache
        - custom cache.

## **Monitoring and Security**

- - You can specify a key stored in the [AWS Key Management Service](https://tutorialsdojo.com/aws-key-management-service-aws-kms/) to encrypt your artifacts.
    - CodeBuild provides security and separation at the infrastructure and execution levels.
    - You can use Amazon [[CloudWatch]] to watch your builds, report when something is wrong, and take automatic actions when appropriate.
    - You can monitor your builds at two levels:
        - At the project level: These metrics are for all builds in the specified project only.
        - At the AWS account level: These metrics are for all builds in one account
    - _ProjectName_ is the only AWS CodeBuild metrics dimension. If it is specified, then the metrics are for that project. If it is not specified, then the metrics are for the current AWS account.

## **AWS CodeBuild Pricing**

- - You are charged for compute resources based on the duration it takes for your build to execute. The per-minute rate depends on the compute type you use.