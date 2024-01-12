# Terraform-on-AWS
Deploying  a Wordpress application into AWS.

### What is Terraform?
* Terraform  is an infrastructure as code tool that lets you provision resources safely and efficiently. This is defined in human-readable configuration files that you can version, reuse and re-share. It creates and manages resources on cloud platforms and other services through their application programming interfaces (APIs). Terraform provide developers the ability to architect and to deploy a reliable, reproducible, and repeatable infrastructure.

<img width="856" alt="images 2" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/c4dae334-73a8-48ad-8d4c-9b4700ed1d6a">

* The Terraform Configuration Language  or Hashicorp's Configuration Language (HCL) is used to declare resources which represent infrastructure objects. These configuration files will tell Terraform the plugins to install, infrastructure to create, and the data to get.

### Terraform Fundamentals
Before diving into the project, we will begin by understanding the Terraform core concepts (providers, modules, and states) and go over the workshop overview. The topics covered in this sections are:

Terraform basics
Folder structure
Local values
Input variables
Output values
Providers
Resources
Data sources
State management
Terraform documentation


### Code Folder Layout
#### Typical Code Folder Layout
 A typical Folder contains a main.tf, outputs.tf, providers.tf, and variables.tf, where

* main.tf contains the core resources of the module
* outputs.tf (Optional) contains the outputs of the module
* variables.tf (Optional) contains the input variables of the module
* providers.tf (Optional) contains the provider configuration which gets covered later on

* Example: main terraform layout
terraform-test
├── main.tf
├── outputs.tf
├── providers.tf
└── variables.tf

### Variables and Outputs
Local values
Local values  are named values that are assigned and can be used throughout your code. Local values can be constant or referenced values. Local values are assigned by creating a set of locals block as shown below:

An example of local values:
locals {
  # Assign the value of 'dev' to environment
  instance_name = "dev-instance"

  # Obtain the instance ID of the 'app_server' EC2 instance
  instance_id   = aws_instance.app_server.id
}

### To use local variables, the format is local.<variable_name>. Here is an example of using a local variable to name the EC2 instance resource.

Note: On AWS, the Name tag is a reserved key and it enables you to provide a name to the given resource. Tags are key/value pairs that act as metadata to organize your AWS resources. Visit Tagging AWS Resources  to learn more about AWS resources tagging and best practices.

resource "aws_instance" "this_server" {
  # ...

  tags = {
    # Using local variable
    Name = local.instance_name,
    "Environment" = "dev"
  }
}

### Input Variables
Input variables  are used to provide parameters for you to customize your Terraform module without altering the module's source code and prevent hard-coding values and enabled you to re-use code

Note: In Advanced Topics, we will briefly discuss Modules

An example of input variables:
variable "app_name" {
  type        = string
  description = "Name  of the application"
  default     = ""
}

### To use input variables, the format is var.<variable_name>. Here is an example of using the input variable to name the EC2 instance resource:

resource "aws_instance" "app_server" {
  # ...

  tags = {
    # Using input variable
    Name = var.app_name,
    "Environment" = "prod"
  }
}

### I can also assign variables using the command line:terraform apply -var="app_name=wordpress-app"

### Output Variables
Output variables  allows you to expose information on the resources so that others Terraform configurations can use it.

An example of output variables
output "instance_tags" {
  value       = aws_instance.this_server.tags
  description = "A mapping of EC2 instance tags"
}


### Providers, Resources, and Data Sources
* Providers
Providers  provide interactions with cloud providers, Software as a Service (SaaS) providers and other Application Programming Interface (API). Each provider provides a set of resources and data sources that Terraform can manage.

Example of an AWS provider:

provider "aws" {
  region = "us-east-1"
}

### Resources
Resources  are the core element in Terraform. Declaring a resource can define one or more infrastructure resource objects such as compute, networking, etc ...

Example of a AWS Simple Storage Service (S3) bucket resource:

resource "aws_s3_bucket" "example" {
  bucket = "my-tf-test-bucket"    
}

Data Sources
Data Sources  allow lookup of resources defined outside of Terraform and provide the attributes found within that resource

Example of a data source lookup of an existing AWS Virtual Private Cloud (VPC)

data "aws_vpc" "selected" {
  id = "vpc-00f0b02721857a89d"
}

### Terraform State
* What is Terraform State?
Terraform stores information about your infrastructure within a state file , typically in a file named terraform.tfstate. Terraform uses state to determine which changes to make to your infrastructure like to modify, add, or remove. Before any operation is applied, Terraform performs a refresh to sync the current infrastructure for any resources' configuration drifts.

* Sample of a Terraform State file
The state file are stored in a JavaScript Object Notation (JSON) format. Below is an example of a state file of a deployment of a VPC resource with some data sources to the us-west-2 region.

{
  "version": 4,
  "terraform_version": "1.4.6",
  "serial": 133,
  "lineage": "cb58f73d-003f-5734-7872-6019650d0d41",
  "outputs": {},
  "resources": [
    {
      "mode": "data",
      "type": "aws_availability_zones",
      "name": "available",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "all_availability_zones": null,
            "exclude_names": null,
            "exclude_zone_ids": null,
            "filter": null,
            "group_names": [
              "us-west-2"
            ],
            "id": "us-west-2",
            "names": [
              "us-west-2a",
              "us-west-2b",
              "us-west-2c",
              "us-west-2d"
            ],
            "state": "available",
            "timeouts": null,
            "zone_ids": [
              "usw2-az2",
              "usw2-az1",
              "usw2-az3",
              "usw2-az4"
            ]
          },
          "sensitive_attributes": []
        }
      ]
    },
    {
      "mode": "data",
      "type": "aws_region",
      "name": "current",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "description": "US West (Oregon)",
            "endpoint": "ec2.us-west-2.amazonaws.com",
            "id": "us-west-2",
            "name": "us-west-2"
          },
          "sensitive_attributes": []
        }
      ]
    },
    {
      "mode": "managed",
      "type": "aws_vpc",
      "name": "default",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 1,
          "attributes": {
            "arn": "arn:aws:ec2:us-west-2:000000000000:vpc/vpc-00f0b02721857a89d",
            "assign_generated_ipv6_cidr_block": false,
            "cidr_block": "10.0.0.0/16",
            "default_network_acl_id": "acl-09dcdd87a8f96ba42",
            "default_route_table_id": "rtb-059654c5165723557",
            "default_security_group_id": "sg-0e43a7c421cc6f39d",
            "dhcp_options_id": "dopt-0869880b486ae393b",
            "enable_dns_hostnames": true,
            "enable_dns_support": true,
            "enable_network_address_usage_metrics": false,
            "id": "vpc-00f0b02721857a89d",
            "instance_tenancy": "default",
            "ipv4_ipam_pool_id": null,
            "ipv4_netmask_length": null,
            "ipv6_association_id": "",
            "ipv6_cidr_block": "",
            "ipv6_cidr_block_network_border_group": "",
            "ipv6_ipam_pool_id": "",
            "ipv6_netmask_length": 0,
            "main_route_table_id": "rtb-059654c5165723557",
            "owner_id": "000000000000",
            "tags": {
              "Name": "terraform-workshop-vpc"
            },
            "tags_all": {
              "Management": "Terraform",
              "Name": "terraform-workshop-vpc"
            }
          },
          "sensitive_attributes": [],
          "private": "eyJzY2hlbWFfdmVyc2lvbiI6IjEifQ==",
          "dependencies": [
            "data.aws_availability_zones.available"
          ]
        }
      ]
    }
  ],
  "check_results": null
}

###  Documentation
* General Documentation
* Terraform Document ( https://developer.hashicorp.com/terraform/docs)
This link provides the general documentations and tutorials for Terraform. It provides information for general use cases.
* Registry
Terraform Registry (https://registry.terraform.io/?ajs_aid=0ab270f5-5386-4fa8-a936-6f0866ba1668&product_intent=terraform  )
This provides a registry of all available providers including documentation on how to utilize them.

Sample: Creating a new aws_vpc resource using terraform registry
* Reference: aws_vpc
* Basic Usage

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}


### Argument References
Reference: aws_vpc  (https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc#argument-reference  )
*These are arguments that the resource require or optionally needs in order to create the target resource.

Example for an optional argument for the VPC resource
cidr_block - (Optional) The IPv4 CIDR block for the VPC. CIDR can be explicitly set or it can be derived from IPAM using

### Attributes Reference
Reference: aws_vpc Attributes ( https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc#attributes-reference )

Resource attributes are exported to be available as reference to other resources, local variables or output variables

### Terraform CLI
Introduction
Terraform provides a command line interface (CLI) that can be called with the terraform command once you've installed Terraform onto your system.

Main commands for Terraform CLI are:
<img width="598" alt="Terraform commands images" src="https://github.com/Gailpositive/Terraform-as-IaC-on-AWS/assets/111061512/bb5e6d2a-a0d3-43be-854c-bd9bef46dd97">


* terraform init
The terraform init command initializes your Terraform working directory. It is the first command you run when writing a new Terraform configuration or when you clone an existing one from a repository as it performs multiple initialization steps to get your current working directory to use Terraform.

* terraform validate
The terraform validate command runs checks to verify that the Terraform configuration in the working directory is syntactically valid, but does not validate remote services like remote state and provider APIs. It is commonly used to validate reusable modules and ensure that the attribute names and value types are generally correct

* terraform plan
The terraform plan command creates and execution plan that provides you a preview of the changes that will be made to your infrastructure (ie. which resources will be created, which will be deleted, and which ones will be modified). If there are no changes to be made, Terraform will report that no changes will be made.

* terraform apply
The terraform apply command executes the actions that are proposed from the Terraform plan. It is best practice to leverage terraform validate and terraform plan prior to running this command so that you can ensure that you're deploying your resources thorough Terraform as intended

* terraform destroy
The terraform destroy command deletes all remote resources that are managed by the current working directory's Terraform configuration. Note that if the resource was created outside of the particular Terraform configuration, it will not be destroyed.
