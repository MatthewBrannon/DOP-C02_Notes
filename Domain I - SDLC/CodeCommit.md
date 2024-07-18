## AWS CodeCommit

- A fully-managed source control service that hosts secure Git-based repositories, similar to Github.
- You can create your own code repository and use Git commands to interact with your own repository and other repositories.
- You can store and version any kind of file, including application assets such as images and libraries alongside your code.
- The AWS CodeCommit Console lets you visualize your code, pull requests, commits, branches, tags and other settings.

## **Concepts**

- An _active user_ is any unique AWS identity [[IAM]] user/role, federated user, or root account) that accesses AWS [[CodeCommit]] repositories during the month. AWS identities that are created through your use of other AWS Services, such as AWS [[CodeBuild]] and AWS [[CodePipeline]], as well as servers accessing CodeCommit using a unique AWS identity, count as active users.
    - A **repository** is the fundamental version control object in [[CodeCommit]]. It’s where you securely store code and files for your project. It also stores your project history, from the first commit through the latest changes.
    - A **file** is a version-controlled, self-contained piece of information available to you and other users of the repository and branch where the file is stored.
    - A **pull request** allows you and other repository users to review, comment on, and merge code changes from one branch to another.
    - An **approval rule** is used to designate a number of users who will approve a pull request before it is merged into your branch.
    - A **commit** is a snapshot of the contents and changes to the contents of your repository. This includes information like who committed the change, the date and time of the commit, and the changes made as part of the commit.
    - In Git, **branches** are simply pointers or references to a commit. You can use branches to separate work on a new or different version of files without impacting work in other branches. You can use branches to develop new features, store a specific version of your project from a particular commit, etc.

## **Repository Features**

- You can share your repository with other users. 
    - If you add AWS tags to repositories, you can set up notifications so that repository users receive email about events, such as another user commenting on code. 
    - You can create triggers for your repository so that code pushes or other events trigger actions, such as emails or code functions.
    - To copy a remote repository to your local computer, use the command ‘git clone’
    - To connect to the repository after the name is changed, users must use the ‘git remote set-url’ command and specify the new URL to use.
    - To push changes from the local repo to the [[CodeCommit]] repository, run ‘git push _remote-name branch-name_’.
    - To pull changes to the local repo from the [[CodeCommit]] repository, run ‘git pull _remote-name branch-name_’.
    - You can create up to 10 triggers for Amazon [[SNS]] or AWS [[Lambda]] for each [[CodeCommit]] repository.
    - You can push your files to two different repositories at the same time.

## **File Features**

- You can organize your repository files with a directory structure.
    - [[CodeCommit]] automatically tracks every change to a file.
    - You can use the AWS Console or AWS CLI and the ‘put-file’ command to add a file or submit changes to a file in a CodeCommit repository.

## **Pull Requests**

- Pull requests require two branches: a source branch that contains the code you want reviewed, and a destination branch, where you merge the reviewed code.
    - Create pull requests to let other users see and review your code changes before you merge them into another branch.
    - Create approval rules for your pull requests to ensure the quality of your code by requiring users to approve the pull request before the code can be merged into the destination branch. You can specify the number of users who must approve a pull request. You can also specify an approval pool of users for the rule.
    - To review the changes on files included in a pull request and resolve merge conflicts, you use the [[CodeCommit]] console, the ‘git diff’ command, or a diff tool.
    - After the changes have been reviewed and all approval rules on the pull request have been satisfied, you can merge a pull request using the AWS Console, AWS CLI, or with the ‘git merge’ command.
    - You can close a pull request without merging it with your code.

## **Commit and Branch Features**

- - If using the AWS CLI, you can use the ‘create-commit’ command to create a new commit for your branch.
    - If using the AWS CLI, you can use ‘create-branch’ command to create a new branch for your repository..
    - You can also use Git commands to manage your commits and branches.
    - Create a new commit to a branch using ‘git commit -m _message_’.
    - Create a branch in your local repo by running the git checkout -b _new-branch-name_ command.

## **Migration from Git repositories to CodeCommit**

- You can migrate a Git repository to a [[CodeCommit]] repository in a number of ways: by cloning it, mirroring it, or migrating all or just some of the branches.
    - You can also migrate your local repository in your machine to [[CodeCommit]].

![AWS CodeCommit](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/01/AWS-CodeCommit1.jpg)

## **High Availability**

- CodeCommit stores your repositories in Amazon [[S3]] and Amazon [[DynamoDB]].

## **AWS CodeCommit** **Security**

- You can transfer your files to and from AWS [[CodeCommit]] using HTTPS or SSH. For HTTPS authentication, you use a static username and password. For SSH authentication, you use public keys and private keys.
    - Your repositories are also automatically encrypted at rest through AWS [[KMS]] using customer-specific keys.
    - You can give users temporary access to your AWS CodeCommit repositories through a number of methods: SAML, Federation, Cross-Account Access, or through third party authorizes with Amazon [[Cognito]].

## **Monitoring**

- CodeCommit uses AWS [[IAM]] to control and monitor who can access your data as well as how, when, and where they can access it. 
    - CodeCommit helps you monitor your repositories via AWS [[CloudTrail]] and AWS [[CloudWatch]].
    - You can use Amazon [[SNS]] to receive notifications for events impacting your repositories. Each notification will include a status message as well as a link to the resources whose event generated that notification.

## **AWS CodeCommit Pricing**

- The first 5 active users per month are free of charge. You also get to have unlimited repositories, with 50 GB-month total worth of storage, and 10,000 Git requests/month at no cost.
    - You are billed for each active user beyond the first 5 per month. You also get an additional 10GB-month of storage per active user, and an additional 2,000 Git requests per active user.
        - A Git request includes any push or pull that transmits repository objects, including a direct edit in the console or through the [[CodeCommit]] APIs.

## **Limits**

- 1,000 repositories by default (no limits upon request).
    - A single blob in a repository cannot be more than 2 GB in size.
    - Total size of your files in a single commit should not be more than 20 MB.
    - An individual file should not exceed 6 MB.