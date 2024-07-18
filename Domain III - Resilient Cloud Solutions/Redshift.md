## Amazon Redshift

- A fully managed, **petabyte-scale data warehouse** service.
- Redshift extends data warehouse queries to your data lake. You can run analytic queries against petabytes of data stored locally in Redshift, and directly against exabytes of data stored in [[S3]].
- RedShift is an OLAP type of DB.
- Currently, Redshift only supports Single-AZ deployments.
- Features
    - Redshift uses **columnar storage**, data compression, and zone maps to reduce the amount of I/O needed to perform queries.
    - It uses a **massively parallel processing** data warehouse architecture to parallelize and distribute SQL operations.
    - Redshift uses machine learning to deliver high throughput based on your workloads.
    - Redshift uses **result caching** to deliver sub-second response times for repeat queries.
    - Redshift automatically and continuously backs up your data to [[S3]]. It can asynchronously replicate your snapshots to [[S3]] in another region for disaster recovery.

## **Components**

- **Cluster** – a set of **nodes**, which consists of a leader node and one or more compute nodes.
        - Redshift creates one database when you provision a cluster. This is the database you use to load data and run queries on your data.
        - You can scale the cluster in or out by adding or removing nodes. Additionally, you can scale the cluster up or down by specifying a different node type.
        - Redshift assigns a 30-minute maintenance window at random from an 8-hour block of time per region, occurring on a random day of the week. During these maintenance windows, your cluster is not available for normal operations.
        - Redshift supports both the [[EC2]]–[VPC](https://tutorialsdojo.com/amazon-vpc/) and [[EC2]]-Classic platforms to launch a cluster. You create a **cluster subnet group** if you are provisioning your cluster in your VPC, which allows you to specify a set of subnets in your VPC.
    - **Redshift Nodes**
        - The _leader node_ receives queries from client applications, parses the queries, and develops query execution plans. It then coordinates the parallel execution of these plans with the compute nodes and aggregates the intermediate results from these nodes. Finally, it returns the results back to the client applications.
        - _Compute nodes_ execute the query execution plans and transmit data among themselves to serve these queries. The intermediate results are sent to the leader node for aggregation before being sent back to the client applications.
        - Node Type
            - **Dense storage (DS)** node type – for large data workloads and use hard disk drive (HDD) storage.
            - **Dense compute (DC)** node types – optimized for performance-intensive workloads. Uses SSD storage.
    - **Parameter Groups – a group of parameters that apply to all of the databases that you create in the cluster. The default parameter group has preset values for each of its parameters, and it cannot be modified.**
- ### **Database Querying Options**
    - Connect to your cluster and run queries on the AWS Management Console with the Query Editor.
    - You can use the Query editor with Redshift clusters enabled and with enhanced VPC routing. Leverage AWS [[Secrets Manager]] to store your cluster credentials and use that with the Query Editor.
    - Connect to your cluster through a SQL client tool using standard ODBC and JDBC connections.
- ### **Enhanced VPC Routing**
    - By using Enhanced VPC Routing, you can use VPC features to manage the flow of data between your cluster and other resources.
    - You can also use VPC flow logs to monitor _COPY_ and _UNLOAD_ traffic.
- ### **RedShift Spectrum**
    - Enables you to run queries against exabytes of data in [[S3]] without having to load or transform any data.
    - Redshift Spectrum supports Enhanced VPC Routing.
    - If you store data in a columnar format, Redshift Spectrum scans only the columns needed by your query, rather than processing entire rows.
    - If you compress your data using one of Redshift Spectrum’s supported compression algorithms, less data is scanned.
- ### **RedShift Streaming Ingestion**
    - Allows you to consume and process data directly from a streaming source to a Redshift cluster using SQL.
    - Streaming ingestion eliminates the need for staging data in Amazon [[S3]], which gives you a low-latency, high-speed ingestion.
    - Valid data source:
        - Amazon [[Kinesis]] Data Streams
            - [Amazon Managed Streaming for Apache Kafka (MSK)](https://tutorialsdojo.com/amazon-managed-streaming-for-apache-kafka-amazon-msk/)
- ### **Redshift ML**
    - Allows you to train and deploy machine learning models using the data stored in your Amazon Redshift cluster through a simple **CREATE MODEL** SQL statement.
    - You can make in-database local inferences using SQL, eliminating the need to move data between Redshift and other storage services like Amazon [[S3]].
    - Redshift ML uses [Amazon SageMaker](https://tutorialsdojo.com/amazon-sagemaker/) Autopilot behind the scenes to find the best model based on your input data.
- ### Redshift Data Sharing
    - Redshift Data Sharing is a secure way to share live data across Redshift clusters within an AWS account, without the need to copy or move data.
    - Data Sharing provides live access to the data so that your users always see the most up-to-date and consistent information as it is updated in the data warehouse.
    - Can be used on Redshift RA3 clusters at no additional cost.
- ### Redshift Cross-Database Query
    - Redshift Cross-database queries provide the ability to query across databases in a Redshift cluster, regardless of which database you are connected to.
    - Available on Redshift RA3 node types at no additional cost.
- ### **Cluster Snapshots**
    - Point-in-time backups of a cluster. There are two types of snapshots: automated and manual. Snapshots are stored in [[S3]] using SSL.
    - Redshift periodically takes incremental snapshots of your data every 8 hours or 5 GB per node of data change.
    - Redshift provides free storage for snapshots that is equal to the storage capacity of your cluster until you delete the cluster. After you reach the free snapshot storage limit, you are charged for any additional storage at the normal rate.
    - Automated snapshots are enabled by default when you create a cluster. These snapshots are deleted at the end of a retention period, which is one day, but you can modify it. You cannot delete an automated snapshot manually.
    - By default, manual snapshots are retained indefinitely, even after you delete your cluster.
    - You can share an existing manual snapshot with other AWS accounts by authorizing access to the snapshot.
    - You can configure Amazon Redshift to automatically copy snapshots (automated or manual) for a cluster to another AWS Region.  For automated snapshots, you can also specify the retention period to keep them in the destination AWS Region. The default retention period for copied snapshots is seven days. 
    - If you store a copy of your snapshots in another AWS Region, you can restore your cluster from recent data if anything affects the primary AWS Region. You can configure your cluster to copy snapshots to only one destination AWS Region at a time.

## **Amazon Redshift** **Monitoring**

- Use the _database audit logging_ feature to track information about authentication attempts, connections, disconnections, changes to database user definitions, and queries run in the database. The logs are stored in [[S3]] buckets.
    - Redshift tracks events and retains information about them for a period of several weeks in your AWS account.
    - Redshift provides performance metrics and data so that you can track the health and performance of your clusters and databases. It uses [[CloudWatch]] metrics to monitor the physical aspects of the cluster, such as CPU utilization, latency, and throughput.
    - _Query/Load performance data_ helps you monitor database activity and performance.
    - When you create a cluster, you can optionally configure a [[CloudWatch]] alarm to monitor the average percentage of disk space that is used across all of the nodes in your cluster, referred to as the _default disk space_ alarm.

## **Amazon Redshift** **Security**

- By default, an Amazon Redshift cluster is only accessible to the AWS account that creates the cluster.
    - Use [[IAM]] to create user accounts and manage permissions for those accounts to control cluster operations.
    - If you are using the [[EC2]]-Classic platform for your Redshift cluster, you must use Redshift security groups.
    - If you are using the [[EC2]]-VPC platform for your Redshift cluster, you must use VPC security groups.
    - When you provision the cluster, you can optionally choose to encrypt the cluster for additional security. Encryption is an immutable property of the cluster.
    - Snapshots created from the encrypted cluster are also encrypted.

### **Amazon Redshift Pricing**

- You pay a per-second billing rate based on the type and number of nodes in your cluster.
    - You pay for the number of bytes scanned by RedShift Spectrum
    - You can reserve instances by committing to using Redshift for a 1 or 3-year term and save costs.