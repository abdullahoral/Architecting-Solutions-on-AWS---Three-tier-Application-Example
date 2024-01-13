### Architecting Solutions on AWS 
### Three-tier Application Example

**Scenario:** 
You are working for a customer that runs their workloads on premises. Your customer has two workloads:

- A three-tier architecture composed of a frontend (HTML, CSS, JavaScript), backend (Apache Web Server and a Java application), and database (MySQL). The three-tier application hosts a dynamic website that accepts user traffic from the internet.
- A data analytics workload that runs Apache Hadoop. The analytics workload analyzes a massive amount of data that stored on premises and it also uses visualization tools to derive insights.

These components are currently running in the data center on physical servers. Currently, if a power outage occurred in the data center, all systems would be brought offline. Because of this issue (in addition to other benefits of the cloud), your customer wants to migrate all components to the cloud and, when possible, use AWS services to replace on-premises components.

### **Solution**
### Architecture Diagram

![Aws Diagram](https://github.com/abdullahoral/Architecting-Solutions-on-AWS---Three-tier-Application-Example/assets/36596429/da9c3f40-11ef-4f8b-9d11-1976e9635be1)

The architecture solution I propose for both the three-tier application and the data analytics workload involves a combination of AWS managed services and the refactoring of the application code to take advantage of cloud-native technologies. Here's how both the solutions work:

### Three-tier application:

1. **Frontend:** Instead of hosting the frontend on a server, we can use Amazon S3 to host the static HTML, CSS, and JavaScript files. This decouples the frontend from the backend and makes it highly available and scalable, as S3 can serve these static files directly to the end users. Route 53 helps to route end users to internet applications by translating domain names into IP addresses. CloudFront uses a global network of edge locations to cache and distribute content closer to end users. This helps reduce the load on origin servers and improves the overall performance of applications.
2. **Backend:** The backend can be refactored into a serverless architecture using AWS Lambda and API Gateway. The Apache Web Server and Java application would be moved to Lambda functions, and API Gateway would be used to route requests to the appropriate Lambda function. This setup allows for automatic scaling and eliminates the need to manage servers. Putting SQS between API Gateway and Lambda is decoupling the API from compute. So that way, if you have a large scaling event and you reach any predefined limits for the Lambda, the messages will be in the queue and will not be lost. Then, Lambda can churn through the messages and catch up.
3. **Database:** The MySQL database can be migrated to Amazon RDS, which is a managed relational database service. It manages time-consuming database administration tasks, leaving more time for application development.

### Data Analytics Workload:

1. **Ingestion:** To ingest data into AWS, Amazon Kinesis Data Firehose can be used. It can capture, transform, and load streaming data into other AWS services like S3, Redshift, or Elasticsearch Service, enabling near real-time analytics with existing business intelligence tools.
2. **Storage:** The ingested data can be stored in Amazon S3, a highly durable, scalable, and reliable storage service.
3. **Processing:** Amazon EMR is a cloud-native big data platform that can be used to process vast amounts of data quickly and cost-effectively. It would be used to replace the onsite Hadoop cluster.
4. **Visualization:** Amazon QuickSight can be used for creating visualizations and deriving insights from the processed data.

### Data Migration:

1. Massive amount of data that stored on premises must be transferred to Cloud. AWS Snowmobile is an exabyte-scale data transfer service used to move large amounts of data to AWS.

This solution leverages the scalability, reliability, and high-availability of AWS's managed services to create a robust, fault-tolerant application and data analytics platform. It also eliminates the need for the customer to manage physical servers, reducing the overhead and risk associated with maintaining an on-premises data center.
