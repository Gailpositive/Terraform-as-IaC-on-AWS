# Leveraging Terraform as the Infrastructure as Code (IaC) tool to deploy a highly available Wordpress application in a single region. It will be configured to leverage a Multi-AZ (data replication across multiple AZs) MySQL database and an auto-scaler based on CPU utilization for web hosting.


<img width="638" alt="images" src="https://github.com/Gailpositive/Terraform-on-AWS/assets/111061512/d57717f6-3790-453e-8f41-58a3f6c1de15">

## Architecture
<img width="707" alt="terraform aws image2 archetectual" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/5b17031e-c2cf-4b04-9c84-41115624df4f">

### The AWS Services used:
* AWS Identity and Access Management  (AWS IAM) - securely manage identities and access to AWS services and resources
* Amazon Virtual Private Cloud  (Amazon VPC) - define and launch AWS resources in a logically isolated virtual environment
* Amazon Elastic Compute Cloud  (Amazon EC2) - provides scalable computing capacity
* Amazon Relational Database Service  (Amazon RDS) - fully managed, open-source cloud database service that allows you to * easily operate and scale your relational database
* Amazon Simple Storage Service  (Amazon S3) - object storage service offering industry-leading scalability, data availability, security, and performance
* Amazon Elastic File System  (Amazon EFS) - serverless, fully elastic file storage
* AWS Simple Systems Manager  (SSM) - operations hub for your AWS applications and resources and a secure end-to-end management solution
* AWS Secrets Manager  - manage, retrieve, and rotate database credentials, tokens, API keys, and other secrets
* AWS Certificate Manager  (ACM) - provision and manage SSL/TLS certificates with AWS services and connected resources
* Amazon CloudFront  - content delivery network (CDN) service built for high performance, security, and developer convenience

* 

