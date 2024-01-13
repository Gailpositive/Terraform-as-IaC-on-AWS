# Deploying WordPress on AWS With Terraform.

### Using Terraform, we're setting up a reliable WordPress app in one place. It'll use a Multi-AZ setup for the database to make data safer and an auto-scaler for web hosting, adjusting based on how much the computer is being used

<img width="638" alt="images" src="https://github.com/Gailpositive/Terraform-on-AWS/assets/111061512/d57717f6-3790-453e-8f41-58a3f6c1de15">

#### At The End Of This Project, I will Be Able To Understand the basic building blocks of Terraform (providers, data sources, resources, etc)
#### Develop Terraform project on Amazon Web Services
#### Getting started into a typical workflow for Terraform
#### Update and deploy changes into my infrastructure environment

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
<img width="959" alt="123 apply" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/3ed54300-b6ab-4c40-8f97-4f93a6481267">
 <img width="936" alt="123b apply completed" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/ec225f0b-f3d7-4fe5-9242-63c27636bfea">

### STEP 6: Create and deploy IAM resources to enable EC2 instances to read from an S3 bucket and grant full access permissions to RDS resources to be created later.

<img width="916" alt="aws vpc image" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/344b68bf-4f96-4143-810d-07e4c707f649">

* Open main.tf in the terraform folder
* Append the content and save
* By updating the main.tf, I am creating IAM roles(aws_iam_role)
* And instance profiles (  aws_iam_instance_profile - an IAM EC2 Instance profile of a IAM role to attach to an EC2 instance )  from the data sources loaded in the previous step
* And performing a terraform apply to deploy the resources into my AWS environment.
* Run the command terraform init to fetch the provider to create resources
* Run the command terraform validate to validate syntax
* Run the command terraform plan to plan the deployment
* Run the command terraform apply to apply the deployment. Type 'yes' to confirm the action
<img width="955" alt="119" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/6063879b-8845-4a36-a6fe-6cd7e72cf5f1">

### Review Deployment
* Navigate to IAM roles to confirm creation resources and inspections of the roles:
* App
<img width="916" alt="124a" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/b9cd1f16-026b-4b1b-a290-aefca60641a0">
* web_hosting
<img width="885" alt="124b" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/2d29eec1-f407-4951-b3d1-e5d6f515175c">

* In the Tag tab,  the role is tagged with Key: Management, Value: Terraform, from previous section that all resources created by Terraform has been configured with this tag.
<img width="914" alt="124c" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/7142f02d-b51d-46b7-9ec1-803fa069fd39">

* To quickly confirm the successful creation of the instance profiles,
* I run the following command: app-profile, web-hosting-profile
* "aws iam get-instance-profile --instance-profile-name app-profile | grep InstanceProfile -q && echo "Successfully created app-profile" || echo "Creation of app-profile was successful"
* aws iam get-instance-profile --instance-profile-name web-hosting-profile | grep InstanceProfile -q && echo "Successfully created web-hosting-profile" || echo "Creation of web-hosting-profile was unsuccessful"

<img width="948" alt="125 instance profile successful" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/282e5a2b-37b0-4088-beb8-61eab6976bda">

### STEP 7: Create and deploy networking resources for hosting an application, involving the creation of a new Virtual Private Cloud (VPC) with all essential resources to run a sample application

<img width="665" alt="networking vpc image" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/119e86c5-6a21-451b-9563-319e493373ab">

### Creating Resources
* aws_vpc - Create a default VPC
* aws_subnet - Create a subnet within a VPC
* aws_internet_gateway - Allow communication between VPC and the internet
* aws_route_table - Create a routing table to control where the network traffic is directed
* aws_main_route_table_association - Set the default network routing table for the default VPC
* aws_route_table_association - Associate the route table to a VPC subnet
* aws_nat_gateway - Create a Network Address Translation (NAT) gateway to allow private subnet to access services
* aws_eip - Create an Elastic IP (EIP) for the NAT Gateways

* Open main.tf in the terraform folder
* Append the content and save
* By updating the main.tf you are creating your VPC network components such as the subnets, internet gateway, route tables, and NAT gateway.
  <img width="937" alt="126 apehend vpc network" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/686bea82-7542-4dcd-a66a-baae9913ba61">

* Run the command terraform validate to validate syntax
* Run the command terraform plan to plan the deployment
* Run the command terraform apply to apply the deployment
<img width="959" alt="127 validate syntax plan the deployment apply deployment was succeefusful" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/5b242a9a-c86f-4c4b-82ba-680a0dd9625b">

### Check VPC deployment components to ensure that the created resources are aligned with my Terraform deployment.

* VPC
<img width="936" alt="128 vpc created" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/96756e74-dc18-4dcb-beff-5b98a53c42fc">

* SUBNETS
 <img width="936" alt="129 private subnet" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/82a42097-d797-4aee-b3c1-b280ee2d0d45">

* ROUTE TABLE
<img width="935" alt="129c subnets created" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/0c64fed6-be49-4cbc-ad07-991dd4f618af">

* Internet gateways
<img width="938" alt="129d" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/a0fae19f-2964-4fc1-807d-d6843e306e17">

* Nat getaways
<img width="922" alt="129e  Nat gateway" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/0b22dc90-6f1f-44a6-8405-7b138f3cd054">

### STEP 7: Create and deploy security resources, including Security Groups and VPC endpoints, within my VPC. VPC endpoints facilitate private connections to supported AWS services and VPC endpoint services through AWS PrivateLink. Amazon VPC instances can communicate with service resources without needing public IP addresses, and the traffic stays within the Amazon network.

<img width="729" alt="security images" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/1921c6d6-5fbe-446d-a6a5-b872abe2d310">

* Open main.tf in the terraform folder
* Append the content and save
* By updating the main.tf, I am  creating security groups to restrict access to my application, database, network file storage, and creating VPC endpoints to enable AWS services like S3 to access my AWS resources through private ingresses.
* Run the command terraform validate to validate syntax
<img width="949" alt="130 security groups append" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/58abf4b0-824d-409e-852b-f299e16a52dc">

* Run the command terraform validate to valid the syntax
* Run the command terraform plan to plan the deployment
* Run the command terraform apply to apply deployment
<img width="944" alt="131 validate plan apply" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/ea568f80-1d2d-47dc-bdd6-7e247d4f932e">
  
### Review Deployment On The VPC Console
* Security groups
<img width="922" alt="132 security groups" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/e1dcb0c0-1989-4c6a-9c33-5eeb756bf825">

* Endpoints
<img width="921" alt="132b endpoint" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/559f0b05-a942-4a1c-9584-b66d1e269fce">

### STEP 8: Create and deploy Resources to host  Application: RDS MySQL database, a load balancer, an autoscaling group, an EFS file system, an S3 bucket, and many more resources,

<img width="707" alt="terraform aws image2 archetectual" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/14393666-8337-4b2b-9102-8ae8659d3127">

* Creating Resources
* aws_db_subnet_group - Create a subnet group for database across availability zones
* random_password - Create a random password
* aws_secretsmanager_secret - Create a Secrets Manager entry
* aws_secretsmanager_secret_version - Create a value for the Secrets Manager entry
* aws_db_instance - Create a MySQL database
* aws_efs_file_system - Create an EFS volume
* aws_efs_mount_target - Create a configuration where the EFS volume to a set of instances
* aws_instance - Create an EC2 that bootstrap the Wordpress application
* aws_ami_copy - Create a copy of the Amazon Machine Image (AMI) being used
* aws_lb - Create a application load balancer (ALB) for the Wordpress web application
* aws_launch_template - Create a launch template that hosts the Wordpress web application
* aws_autoscaling_group - Create a auto-scaler group that scale the Wordpress web application instances
* aws_autoscaling_policy - Create a auto scaling policy
* aws_cloudwatch_metric_alarm - Create a CloudWatch Metric Alarm
* aws_lb_target_group - Create a target group to attach the load balancer for the Wordpress web application
* aws_lb_listener - Create a load balancer listener for the ALB
* tls_private_key - Create a TLS private key
* tls_self_signed_cert - Create a public TLS certificate signed by the private key
* aws_acm_certificate - Create an ACM certificate entry with the TLS public certificate and private key
* aws_s3_bucket - Create a private S3 Bucket
* aws_s3_bucket_public_access_block - Create a public access block which disable public access for the private S3 Bucket
* aws_s3_object - An object of S3, typically represent data similar file
* aws_cloudfront_distribution - Create a CloudFront distribution for the Wordpress web application
* aws_cloudfront_cache_policy - Create a CloudFront cache policy for the Wordpress web application

* Open main.tf in the terraform folder
* Append the content and save
<img width="954" alt="133" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/27742096-8d2f-436d-a453-d3640045c23d">

* Run the command terraform validate to valid the syntax
* Run the command terraform plan to plan the deployment
* Run the command terraform apply to apply deployment
<img width="959" alt="134 application installed" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/2b756a62-0172-4b11-b29c-94ad671d0c55">

### STEP 9: Create Outputs for Application: Exposing the endpoints of the Load Balancer and  CloudFront distribution, to test if application has been deployed correctly.
* alb_endpoint_uri - Application Load Balancer Uniform Resource Identifier
* alb_endpoint_url - Application Load Balancer Uniform Resource Locator
* cloudfront_endpoint_url - CloudFront Uniform Resource Identifier

* Open outputs.tf in the terraform folder
* Append the content and save file
<img width="944" alt="135 output variables" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/ea36507c-bba1-40f2-be9d-a73721376304">

* Run the command terraform validate to valid the syntax
* Run the command terraform plan to plan the deployment
* Run the command terraform apply to apply deployment
<img width="953" alt="136" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/81110ee1-e68e-4591-b51a-13da49255bb2">

### STEP 10: Validate Application Deployment
* Successful comlipetion of all steps will give an output like this
* Copy the value from cloudfront_endpoint_url output and enter it on a new tab in current browser
<img width="810" alt="output validation" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/20bc55cb-c613-424e-80db-767ed8196254">

* Verify application webpage
* Application confirmed
<img width="958" alt="137 url on broswer" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/0efb3f39-6f65-4158-a9c3-aee03d9d431b">
<img width="948" alt="137b" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/b767c5d0-5383-48e7-9e73-7ca2b721bc48">

### STEP 11: Inspect State file
* Open terraform.tfstate in the terraform folder
* Preview it
<img width="940" alt="138 preview state file" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/1d9519b6-307b-4654-980d-0b09922e5630">

### STEP 12: Destroy project automatedly
* Run the command terraform destroy to destroy the deploymen
<img width="960" alt="139 destroy terraform and clean up cloudformation" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/8fe52913-cde6-4a8e-9b37-d82c174c2965">


### Summary
Workshop Completion
Congratulations on completing the workshop! By completing this workshop, you were able to:



What can you do with Terraform in the future
Create a continuous integration and continuous deployment CI/CD pipeline
Use Terraform to create, provision, and bootstrap a demo across various providers, clone new environments or regions with minor adjustments
Ease integration or experiment of new services like Elastic Kubernetes Service (EKS) clusters into a Terraform managed deployment


#### Bonus Challenge: Inspect the tfstate file and locate some security issue(s) .
Hint #1 - Look at the local variable code block for a sensitive value in main.tf and inspect the terraform.tfstate
Hint #2 - There is another sensitive value, see if you can locate it in the terraform.tfstate
