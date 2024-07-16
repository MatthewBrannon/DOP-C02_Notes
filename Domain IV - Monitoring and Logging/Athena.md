## Amazon Athena Cheat Sheet

- An interactive query service that makes it easy to analyze data **directly** in [[S3]] and other data sources using [SQL](https://tutorialsdojo.com/azure-sql/).

## **Features**

- Athena is **serverless**.
- Has a built-in query editor.
- Uses _Presto_, an open source, distributed SQL query engine optimized for low latency, ad hoc analysis of data.
- Athena supports a wide variety of data formats such as CSV, JSON, ORC, Avro, or Parquet.
- Athena automatically executes queries in **parallel**, so that you get query results in seconds, even on large datasets.
- Athena uses Amazon S3 as its underlying data store, making your data highly available and durable.
- Athena integrates with [Amazon QuickSight](https://tutorialsdojo.com/amazon-quicksight/) for easy data visualization.
- Athena integrates out-of-the-box with [AWS Glue](https://tutorialsdojo.com/aws-glue/).
Athena uses a managed **Data Catalog** to store information and schemas about the databases and tables that you create for your data stored in [[S3]].

## **Queries**

- You can query geospatial data.
- You can query different kinds of logs as your datasets.
- [[Athena]] stores query results in [[S3]].
- [[Athena]] retains query history for 45 days.
- [[Athena]] does support User-Defined Functions (UDFs). UDFs in [[Athena]] allow you to create custom functions to process records or groups of records. They are executed with [[Lambda]] when used in an Athena query. However, [[Athena]] only supports scalar UDFs, which process one row at a time and return a single column value.
- Athena supports both simple data types such as INTEGER, DOUBLE, VARCHAR and complex data types such as MAPS, ARRAY, and STRUCT.
- Athena supports querying data in [[S3]] Requester Pays buckets.

## Athena Federated Queries

- Allows you to query data sources other than S3 buckets using a data connector.
- A data connector is implemented in a [[Lambda]] function that uses Athena Query Federation SDK.
- There are pre-built connectors available for some popular data sources, such as:
    - MySQL, PostgreSQL, Oracle, SQL Server databases
    - [[DynamoDB]]
    - Amazon Managed Streaming for Apache Kafka (MSK)
    - [Amazon RedShift](https://tutorialsdojo.com/amazon-redshift/)
    - Amazon OpenSearch
    - [[CloudWatch]] Logs and CloudWatch metrics
    - Amazon DocumentDB
    - Apache Kafka
- You can write your own data connector using the Athena Query Federation SDK if your data source is not natively supported by Athena.
- You may also customize the pre-built connectors to fit your use case.

## Optimizing query performance

- Data partitioning. For instance, partitioning data based on column values such as date, country, and region makes it possible to limit the amount of data that needs to be scanned by a query.
- Converting data format into columnar formats such as Parquet and ORC
- Compressing files
- Making files splittable. [[Athena]] can read a splittable file in parallel; thus, the time it takes for a query to complete is faster.
    - **AVRO**, **Parquet**, and **Orc** are splittable files regardless of the compression codec used
    - Only text files (TSV, CSV, JSON, and custom SerDes for text) compressed with **BZIP2** and **LZO** are splittable.

## Cost controls

- You can create **_workgroups_** to isolate queries for teams, applications, or different workloads and enforce cost controls.
- There are two types of cost controls available in a workgroup:
    - **Per-query limit –** specifies a threshold for the total amount of data scanned per query. Any query running in a workgroup is canceled once it exceeds the specified limit. Only one per-query limit can be created in a workgroup.
    - **Per-workgroup limit** – this limits the total amount of data scanned by all queries running within a specific time frame. You can establish multiple limits based on hourly or daily data scan totals for queries within the workgroup.

## **Amazon Athena Security**

- Control access to your data by using [[IAM]] policies, access control lists, and S3 bucket policies.
- If the files in the target [[S3]]bucket are encrypted, you can perform queries on the encrypted data itself.

## **Amazon Athena Pricing**

- You pay only for the queries that you run. You are charged based on the amount of data scanned by each query.
- You are not charged for failed queries.
- You can get significant cost savings and performance gains by compressing, partitioning, or converting your data to a columnar format because each of those operations reduces the amount of data that Athena needs to scan to execute a query.