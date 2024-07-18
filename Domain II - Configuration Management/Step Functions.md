## AWS Step Functions

- AWS Step Functions is a web service that provides **serverless orchestration** for modern applications. It enables you to coordinate the components of distributed applications and microservices using visual workflows.
## **Concepts**

- Step Functions are based on the concepts of **tasks** and **state machines**.
        - A task performs work by using an activity or an AWS [[Lambda]] function, or by passing parameters to the API actions of other services.
        - A finite state machine can express an algorithm as a number of states, their relationships, and their input and output.
    - You define state machines using the **JSON-based Amazon States Language**.
    - A state is referred to by its _name_, which can be any string, but which must be unique within the scope of the entire state machine. An instance of a state exists until the end of its execution.
        - There are 8 types of states:
            - Task state – represents a single unit of work done by your state machine. It could be making API calls to AWS services, such as running an Amazon [[Athena]] query, writing to a [[DynamoDB]] table, or invoking a [[Lambda]] function to perform some custom processes.
            - Choice state – Make a choice between branches of execution
            - Fail state – Stops execution and marks it as failure
            - Succeed state – Stops execution and marks it as a success
            - Pass state – Simply pass its input to its output or inject some fixed data
            - Wait state – Provide a delay for a certain amount of time or until a specified time/date
            - Parallel state – Begin parallel branches of execution
            - Map state – Adds a for-each loop condition
        - Common features between states
            - Each state must have a _Type_ field indicating what type of state it is.
            - Each state can have an optional _Comment_ field to hold a human-readable comment about, or description of, the state.
            - Each state (except a Succeed or Fail state) requires a _Next_ field or, alternatively, can become a terminal state by specifying an **_End_** field.
    - **Activities** enable you to place a task in your state machine where the work is performed by an **_activity worker_** that can be hosted on Amazon [[EC2]], Amazon [[ECS]], or mobile devices.
    - Activity tasks let you assign a specific step in your workflow to code running in an activity worker. Service tasks let you connect a step in your workflow to a supported AWS service.
    - With **Transitions**, after executing a state, AWS Step Functions uses the value of the _Next_ field to determine the next state to advance to. States can have multiple incoming transitions from other states.
    - State machine data is represented by JSON text. It takes the following forms:
        - The initial input into a state machine
        - Data passed between states
        - The output from a state machine
    - Individual states receive JSON as input and usually pass JSON as output to the next state.
		![AWS Step Functions Diagram 1](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2019/05/AWS-Step-Functions-Diagram-1.jpg)
- A **state machine execution** occurs when a state machine runs and performs its tasks. Each Step Functions state machine can have multiple simultaneous executions.
    - **State machine updates** in AWS Step Functions are **eventually consistent**.
    - By default, when a state reports an error, AWS Step Functions causes the execution to **fail entirely**.
        - Task and Parallel states can have a field named _Retry_ and _Catch_ to retry an execution or to have a fallback state.
    - The Step Functions console displays a graphical view of your state machine’s structure, which provides a way to visually check a state machine’s logic and monitor executions.

## **Features**

- Using Step Functions, you define your **workflows as state machines**, which transform complex code into easy to understand statements and diagrams.
    - Step Functions provides ready-made steps for your workflow called **states** that implement basic service primitives for you, which means you can remove that logic from your application. States are able to:
        - pass data to other states and microservices,
        - handle exceptions,
        - add timeouts,
        - make decisions,
        - execute multiple paths in parallel,
        - and more.
    - Using Step Functions **service tasks**, you can configure your Step Functions workflow to call other AWS services.
    - Step Functions can coordinate any application that can make an **HTTPS** connection, regardless of where it is hosted—Amazon [[EC2]] instances, mobile devices, or on-premises servers.
    - AWS Step Functions coordinates your existing [[Lambda]] functions and microservices and lets you modify them into new compositions. The tasks in your workflow can run anywhere, including on instances, containers, functions, and mobile devices.
    - Nesting your Step Functions workflows allows you to build larger, more complex workflows out of smaller, simpler workflows.
    - Step Functions keeps the logic of your application strictly separated from the implementation of your application. You can add, move, swap, and reorder steps without having to make changes to your business logic.
    - Step Functions maintains the state of your application during execution, including tracking what step of execution it is in, and storing data that is moving between the steps of your workflow. You won’t have to manage state yourself with data stores or by building complex state management into all of your tasks.
    - Step Functions automatically handles errors and exceptions with **built-in try/catch and retry**, whether the task takes seconds or months to complete. You can automatically retry failed or timed-out tasks, respond differently to different types of errors, and recover gracefully by falling back to the designated cleanup and recovery code.
    - Step Functions has **built-in fault tolerance and maintains service capacity across multiple Availability Zones in each region**, ensuring high availability for both the service itself and for the application workflow it operates.
    - Step Functions **automatically scales** the operations and underlying compute to run the steps of your application for you in response to changing workloads.
    - AWS Step Functions has a 99.9% SLA.
    - AWS Step Functions enables access to metadata about workflow executions so that you can easily identify related resources. For example, your workflow can retrieve its execution ID or a timestamp indicating when a task started. This makes it easier to correlate logs for faster debugging and to measure workflow performance data.
    - It also supports callback patterns. Callback patterns automate workflows for applications with human activities and custom integrations with third-party services.
    - AWS Step Functions supports workflow execution events, which makes it faster and easier to build and monitor event-driven, serverless workflows. Execution event notifications can be automatically delivered when a workflow starts or completes through [[CloudWatch]] Events, reaching targets like AWS [[Lambda]], Amazon [[SNS]], Amazon [[Kinesis]], or AWS Step Functions for automated response to the event. 
## **Standard vs. Express Workflow**

- Standard Workflow
	- Designed for long-running business processes that require durable state storage.
	- Can execute for a maximum of one year.
	- Provides automatic retries, error handling, and human approval steps.
	- Offers many advanced features, including execution history tracking for up to 90 days and timeouts.
	- Suitable for complex and fault-tolerant workflows that require coordination across multiple services.
- Express Workflow
	- Designed for high-volume, short-duration workflows that can be completed within minutes or seconds.
	- It can execute for a maximum of five minutes.
	- Provides no durable state storage, no automatic retries, no error handling, and no human approval steps.
	- Optimized for low-latency and high-throughput use cases, such as event-driven serverless applications.
	- Can integrate with AWS Step Functions service integrations, AWS [[Lambda]], and HTTP/HTTPS endpoints.
	- Provides a simpler and more lightweight workflow execution environment with lower per-state costs.

## **How Step Functions Work**

- Orchestration centrally manages a workflow by breaking it into multiple steps, adding flow logic, and tracking the inputs and outputs between the steps.
    - As your applications execute, Step Functions maintains application state, tracking exactly which workflow step your application is in, and stores an event log of data that is passed between application components. That means that if networks fail or components hang, your application can pick up right where it left off.![AWS Step Functions Diagram 2](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2019/05/AWS-Step-Functions-Diagram-2.jpg)
- Amazon States Language
    - A JSON-based, structured language used to define your state machine, a collection of states, that can do work (_Task states_), determine which states to transition to next (_Choice states_), stop an execution with an error (_Fail states_), and so on.
    - When creating a state machine, only the _States_ field is required in the JSON document.
- Security and Monitoring
    - AWS Step Functions can execute code and access AWS resources (such as invoking an AWS [[Lambda]] function). To maintain security, you must grant Step Functions access to those resources by using an [[IAM]] role.
    - Step Functions is a HIPAA eligible service and is also compliant with SOC, PCI, and FedRAMP.
    - Step Functions delivers real-time diagnostics and dashboards, integrates with Amazon [[CloudWatch]] and AWS [[CloudTrail]], and logs every execution, including overall state, failed steps, inputs, and outputs.

## **AWS Step Functions** **Pricing**

- Step Functions count a state transition each time a step of your workflow is executed. You are charged for the total number of state transitions across all your state machines, including retries.

## **Common Use Cases**

- Step Functions can help ensure that long-running, multiple ETL jobs execute in order and complete successfully, instead of manually orchestrating those jobs or maintaining a separate application.
    - By using Step Functions to handle a few tasks in your codebase, you can approach the transformation of monolithic applications into microservices in a series of small steps.
    - You can use Step Functions to easily automate recurring tasks such as patch management, infrastructure selection, and data synchronization, and Step Functions will automatically scale, respond to timeouts, and retry failed tasks.
    - Use Step Functions to combine multiple AWS [[Lambda]] functions into responsive serverless applications and microservices, without having to write code for workflow logic, parallel processes, error handling, timeouts, or retries.
    - You can also orchestrate data and services that run on Amazon [[EC2]] instances, containers, or on-premises servers.