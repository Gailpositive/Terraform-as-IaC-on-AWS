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

## Heads Up
* Quick links to the AWS console are using the us-east-1 as the base region. It is highly recommended that the instance profile AWSCloud9SSMInstanceProfile and the IAM role AWSCloud9SSMAccessRole must not exist on the account prior starting this project.

* Check if the instance profile AWSCloud9SSMInstanceProfile exist by entering the following in an AWS CloudShell terminal. If it returns a non-empty response, then the instance profile AWSCloud9SSMInstanceProfile exist.
* aws iam list-instance-profiles | grep AWSCloud9SSMInstanceProfile
### You can delete the instance profile at your own risk, by entering following in an AWS CloudShell terminal.
* aws iam detach-role-policy --role-name AWSCloud9SSMAccessRole --policy-arn $(aws iam list-attached-role-policies --role-name AWSCloud9SSMAccessRole --output text | awk '{ print $2 }')
* aws iam remove-role-from-instance-profile --instance-profile-name AWSCloud9SSMInstanceProfile --role-name AWSCloud9SSMAccessRole
* aws iam delete-role --role-name AWSCloud9SSMAccessRole
* aws iam delete-instance-profile --instance-profile-name AWSCloud9SSMInstanceProfile

## STEP 1: Download Code 
* Download yaml file into my local machine

## STEP 2: Create Stack On AWS Cloudformation
* On AWS console, open Cloudformation
* Create stack → With new resources (standard)
<img width="931" alt="101 create stack" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/ba912dc5-e523-448a-8b11-7d52fec38bbb">

* Click on template is ready
<img width="930" alt="102" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/449afe11-38a1-4846-a128-dbb41d437a8d">

* Upload the template file
<img width="938" alt="103" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/5122d8ab-73c0-46bb-8572-82b823431726">

* Name the stack
<img width="943" alt="104" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/13846923-ff96-4127-8cc2-ea6c2635324b">

* Leave Configure stack options as defualt
<img width="917" alt="105" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/851d76e5-15c6-45a2-973d-ef221c4cf572">

* Review
<img width="919" alt="106" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/b8ae5fc0-2f34-4ce8-a93d-26dd357f9fe5">

* Accept acknowledgement and submit
<img width="955" alt="107" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/da827578-0b66-4531-a58f-1c26762608db">

* Stack created
<img width="930" alt="108" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/1192e20e-920d-4c70-bfaa-0f4e74c708ba">

### STEP 3: Launching Cloud9 Workspace, Setting Up My IDE And 
* On cloud9 environment
* Open cloud9 IDE
<img width="928" alt="109" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/5263d7fe-17e0-4a73-a632-d790285bbbb3">
<img width="956" alt="110" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/f9b1c645-eeb1-40f7-9b67-07c7329c3568">

### Finalize Cloud9 Configuration
* Select file to unzip
<img width="950" alt="111" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/1b84a88c-2cf4-4426-817c-9d642b5af648">
* Download/unzip the file with the command "unzip terraform.zip -d terraform"
<img width="900" alt="112" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/01f37809-a621-4ff3-9cd9-cf2136eda3b7">
* Close the welcome tab and lower terminal work area
* Open a new terminal tab (Windows -> New Terminal) in the main work area

### STEP 4: Update IAM Setting For My Workspace
* In Cloud9 IDE, click on the gear icon (top right-hand corner)
* Select AWS Settings
* Turn off AWS Managed temporary credentials
* Close the Preferences tab
<img width="967" alt="113b" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/519e7333-7677-45fe-876f-0aab7e400ef7">
* To confirm I am using the IAM role attached to the environment EC2 instance,
* I run the command "aws sts get-caller-identity --no-cli-pager"
<img width="929" alt="114" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/d930c373-71ee-4f7d-8ad1-80feebaefd55">

 ### STEP 5: Create And Update Input Variables, Provider, Local Values, And Data Sources
* cd terraform
<img width="948" alt="115" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/a871b01d-307e-487a-a420-f8456067cfa0">

* Open variables.tf in the terraform folder
* Update the content and save
* Creating three input variables in the variable.tf file which define the number of availability zones, namespace, and VPC CIDR block to be used for this Terraform project's deployment
<img width="957" alt="116" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/e7f0065e-6001-40b4-a2f7-d3d488fd59ae">

* Open providers.tf in the terraform folder
* Update the content and save
* I am requiring that the provider used in this project to be aws, the version of the hashicorp/aws provider must be a version greater than or equal to 5.0, and that all resources created by Terraform will be tagged with Key: Management, Value: Terraform.
<img width="950" alt="117" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/d9de3089-35aa-4c81-9145-b70f9fad7075">

* Open main.tf in the terraform folder
* Update the content and save
* The update to main.tf begins by creating local variables that define the VPC availability zone and CIDR block values, RDS instance values, EC2 instance type, and credentials for the Wordpress Admin user.
* Next, the file defines data sources to look up definitions of the given AWS environment such as the current region, list of availability zones, ARNs of IAM policies, etc which will be used to create AWS resources.
<img width="944" alt="118" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/b9b6b39e-3aff-417e-8172-b0961221ca15">


* run terraform init command 
<img width="940" alt="120 terraform init" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/cd71690e-0bf7-411c-a067-d063c212eaba">

* Run terraform validate command  to validate syntax
 <img width="908" alt="121 terraform validate" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/b049de92-0eb4-4616-b315-1a6affe9448a">

* run terraform plan command  to plan deployment
<img width="938" alt="122 plan" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/47bebe58-3fd7-46bb-ad2a-01d0139e1044">
<img width="955" alt="122b" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/d8579069-912f-4693-a746-8d0ee72b4a60">

* run terraform apply command to execute deployment
<img width="959" alt="123 apply" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/42e2e904-cd11-45d8-aa97-ce63db197358">

* Open main.tf in the terraform folder
* Update the content and save
* Updating main.tf begins by creating local variables that define the VPC availability zone and CIDR block values,
* RDS instance values,
* EC2 instance type,
* And credentials for the Wordpress Admin user.
* Next, the file defines data sources to look up definitions of the given AWS environment such as:
* The current region,
* List of availability zones,
*  ARNs of IAM policies, etc which will be used to create AWS resources.
<img width="936" alt="123b apply completed" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/ec225f0b-f3d7-4fe5-9242-63c27636bfea">
<img width="955" alt="119" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/6063879b-8845-4a36-a6fe-6cd7e72cf5f1">

<img width="916" alt="aws vpc image" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/344b68bf-4f96-4143-810d-07e4c707f649">

### STEP 6: Create Resources for IAM that will allow EC2 instances to read from an S3 bucket, as well get full access permissions to the RDS resources that will create later on.
* Open main.tf in the terraform folder
* Append the content and save
* By updating the main.tf, I am creating IAM roles(aws_iam_role)
* And instance profiles (  aws_iam_instance_profile - an IAM EC2 Instance profile of a IAM role to attach to an EC2 instance )  from the data sources loaded in the previous step
* And performing a terraform apply to deploy the resources into my AWS environment.
<img width="916" alt="124a" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/b9cd1f16-026b-4b1b-a290-aefca60641a0">

<img width="885" alt="124b" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/2d29eec1-f407-4951-b3d1-e5d6f515175c">


  
