# virga-task

![alt text](./infra.png)


# Annotations:

- Load Balancer: Distributes incoming traffic across the front-end EC2 instances for scalability and fault tolerance.
- Auto Scaling Group: Manages the number of front-end and back-end EC2 instances based on demand, ensuring availability and scalability.
- API Gateway: Acts as the central entry point for the back-end services, enabling API management, request routing, and authentication.
- Lambda: Executes the back-end functions for data retrieval and aggregation based on user input, connecting to various data sources.
- RDS: Stores structured data and provides high availability with features like Multi-AZ deployment and read replicas.
- S3: Stores unstructured or semi-structured data, serving as a scalable and durable storage solution.

- Optional: 
Other Data Sources: an additional data sources beyond RDS and S3 that the back-end functions can connect to, providing flexibility for integrating with external systems or services.


**Accessing Data Stores using a Common Pattern:**

To ensure the application can access any data store using a common pattern, we can utilize AWS Lambda functions as the glue between the application servers and the data stores. Each data store can have a corresponding Lambda function acting as an API gateway for the specific store. The Lambda functions can handle the communication and data retrieval from the respective data stores, abstracting the complexity for the front-end developers. This approach allows for a consistent interface and decouples the application from the data stores.

- Implement a common data access pattern, such as the Repository Pattern, to abstract the data sources and provide a consistent interface for the application to access any data store.

- Create separate data access modules for each data store, encapsulating the logic required to retrieve data. This modular approach allows for easy integration of new data sources in the future

**Handling Spikes of Heavy Traffic and Scaling Strategies:**

To handle spikes of heavy traffic, we can utilize several AWS services:

- Auto Scaling Groups: 
The application servers can be launched in an Auto Scaling group to handle increased load. The group's scaling policies can be configured to add additional instances based on CPU or request metrics, ensuring the application scales horizontally during peak traffic.

- Elastic Load Balancer (ELB): An ELB can be placed in front of the application servers to distribute incoming traffic evenly across the instances. This helps to improve availability and handle the load during high traffic periods.

- Database Scaling: Depending on the specific data stores used (RDS, DynamoDB, etc.), scaling strategies like read replicas, provisioned capacity, or on-demand scaling can be implemented to handle increased database traffic.

**Monitoring and Maintenance Tools/Processes:**

To ensure availability and quickly identify/address issues, the following tools and processes can be employed:

- Amazon CloudWatch: Monitoring metrics such as CPU utilization, network traffic, and application-level metrics can be captured using CloudWatch. Alarms can be set up to notify administrators when predefined thresholds are breached.
In addition, self hosted Grafana and Prometheus could be to get all metrics and buld custom alert based on that.

- AWS CloudTrail: Logging and auditing of API calls made by the application can be achieved using CloudTrail. This helps in troubleshooting and identifying any unauthorized access or potential issues.


- Terraform: Infrastructure can be defined as code using Terraform, enabling consistent and repeatable deployments. This allows for quick and smooth deployment of new versions while minimizing manual errors.

- AWS Database Migration Service: If data migrations are required during application updates, the Database Migration Service can assist in a smooth transition without downtime.

- Regular Backups: Implement regular backups for the databases using automated backup features available in RDS or DynamoDB, ensuring data durability and enabling point-in-time recovery if needed.
Set up automated backup mechanisms for critical data stores to prevent data loss in case of failures or disasters.

- Establish disaster recovery plans and implement strategies like multi-region replication or automated failover to ensure high availability and minimize downtime.

**Continuous Integration and Deployment:**

For Ci/Cd i would use any modern tool such as GitHub Actions or GitLab CI to build, test the image that being pushed to AWS ECR and after that deployed to relevant environment. 

Utilize a robust CI/CD (Continuous Integration/Continuous Deployment) pipeline to automate the deployment process and ensure smooth and quick releases of new versions.
Integrate the CI/CD pipeline with version control systems like Git, triggering automatic builds and deployments whenever changes are pushed to the repository.
Implement automated testing, including unit tests, integration tests, and end-to-end tests, to ensure the stability and correctness of the application before and after deployment.

**Security and Access Control**

- Implement appropriate security measures, including encrypted communication (HTTPS), secure user authentication, and authorization mechanisms, to protect user data and prevent unauthorized access.

- Apply the principle of least privilege, granting the web application's back-end only the necessary permissions to access the required data sources. This reduces the risk of data breaches or accidental access to sensitive information.
