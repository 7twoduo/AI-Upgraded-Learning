Bedrock RAG Chatbot Infrastructure with Terraform

This project provisions a Retrieval-Augmented Generation (RAG) chatbot system on AWS using Terraform. It builds the networking, storage, Bedrock Knowledge Base, Lambda backend, API Gateway, and optional website hosting stack.

The goal of this project is to:

store source documents in S3

create a Bedrock Knowledge Base backed by S3 Vectors

ingest documents into the knowledge base

expose a chat API through API Gateway + Lambda

optionally host a frontend using either:

EC2 + Nginx

S3 + CloudFront + Route 53

Architecture Overview

This Terraform project creates the following:

Core RAG components

S3 bucket for RAG documents

S3 object uploads from the local RAG_Documents/ folder

S3 Vectors Vector Bucket

S3 Vectors Index

Bedrock Knowledge Base

Bedrock Data Source pointing to the documents bucket

Ingestion job trigger to sync documents into the knowledge base

Compute and API

Lambda function for chatbot logic

IAM roles and policies for Lambda and Bedrock

API Gateway REST API with:

POST /chat

CORS support via OPTIONS /chat

stage throttling

Networking

Custom VPC

Public subnet

Private subnet

Internet Gateway

Route tables and associations

Optional hosting

This project supports two website hosting options:

Option 1: EC2 hosting

EC2 instance

Security group

Nginx installation via user data

serves index.html

Option 2: S3 + CloudFront hosting

static website bucket

CloudFront distribution

Origin Access Control (OAC)

Route 53 hosted zone and DNS records

ACM certificate

bucket policy for CloudFront access

Project Structure

Based on your folder layout, the repo looks like this:

.terraform/ — Terraform working directory

modules/ — reusable Terraform modules

RAG_Documents/ — documents uploaded into the RAG S3 bucket

.terraform.lock.hcl — dependency lock file

debug.log — debug output/logs

index.html — frontend page

lambda_function.py — Lambda handler code

lambda.zip — packaged Lambda zip

main.tf — main Terraform infrastructure

output.tf — outputs

var.tf — variables

Prerequisites

Before using this project, make sure you have:

Terraform ~> 1.14.5

AWS CLI installed and configured

an AWS account with permissions for:

VPC

S3

IAM

Lambda

API Gateway

CloudFront

Route 53

Bedrock

Cloud Control API

Bedrock model access enabled in your AWS account

a registered domain if you want to use the CloudFront + Route 53 hosting option

Provider Versions

This project uses:

Terraform: ~> 1.14.5

AWS Provider: 6.33.0

Backend Configuration

Terraform state is stored remotely in S3:

bucket: backend-extra-unique-1

key: terraform.tfstate

region: us-east-1

Important

The backend bucket must already exist before running terraform init.

Terraform will not create the backend bucket automatically from this configuration.

Main Features
1. RAG document storage

A dedicated S3 bucket stores documents from the local RAG_Documents/ folder.

Terraform recursively uploads all files using fileset().

2. Bedrock Knowledge Base

The project creates:

an S3 Vectors vector bucket

an S3 Vectors index

a Bedrock Knowledge Base configured with:

vector storage

Titan embedding model

S3 Vectors backend

3. Bedrock data source ingestion

The S3 bucket is attached as a Bedrock data source, and Terraform triggers an ingestion job using local-exec.

4. Lambda chatbot backend

The Lambda function is packaged from lambda_function.py and configured with:

KB_ID

CLAUDE_MODEL_ID

It is intended to:

retrieve relevant context from the knowledge base

invoke the chosen Bedrock model

return the generated response to the API caller

5. API Gateway

API Gateway exposes the Lambda through:

POST /chat

It also includes:

CORS support

throttling limits

6. Website hosting options

The frontend can be hosted in two ways:

EC2 hosting

Set first_option = true

This creates:

EC2 instance

security group

Nginx web server

serves index.html

S3 + CloudFront hosting

Set second_option = true

This creates:

S3 static content bucket

CloudFront distribution

Route 53 DNS records

ACM certificate

bucket access policy for CloudFront

Variables

Your var.tf likely contains the input variables for values such as:

region

vpc_name

vpc_cidr

public_subnet_cidr

private_subnet_cidr

public_ip

private_ip

public_cidr

embedding_model_arn

claude_model_arn

registered_domain

root_domain_name

Environment

You should review and update these before deployment.

Example terraform.tfvars

A sample terraform.tfvars could look like this:

region = "us-east-1"

vpc_name = "rag-vpc"
vpc_cidr = "10.0.0.0/16"

public_subnet_cidr = "10.0.1.0/24"
private_subnet_cidr = "10.0.2.0/24"

public_ip = true
private_ip = false

public_cidr = "0.0.0.0/0"

embedding_model_arn = "arn:aws:bedrock:us-east-1::foundation-model/amazon.titan-embed-text-v2:0"
claude_model_arn = "arn:aws:bedrock:us-east-1::foundation-model/anthropic.claude-3-sonnet-20240229-v1:0"

registered_domain = "example.com"
root_domain_name = "example.com"
Environment = "dev"

How to Deploy
1. Initialize Terraform

terraform init

2. Review the execution plan

terraform plan

3. Apply the infrastructure

terraform apply

How the Ingestion Flow Works

Files are placed inside RAG_Documents/

Terraform uploads them into the RAG S3 bucket

Terraform creates:

vector bucket

vector index

Bedrock knowledge base

data source

Terraform runs a Bedrock ingestion job with AWS CLI

The chatbot Lambda can then retrieve data from the knowledge base

Hosting Selection
Use EC2 hosting

Set:

first_option = true
second_option = false

Use S3 + CloudFront hosting

Set:

first_option = false
second_option = true

Outputs

Your output.tf should expose useful values such as:

API Gateway invoke URL

Lambda function name

Knowledge Base ID

CloudFront domain name

website bucket name

VPC ID

subnet IDs

Notes and Caveats
1. Backend bucket must exist first

The S3 backend bucket is required before terraform init.

2. Bedrock access is required

Your AWS account must have access to:

the embedding model

the Claude model

Bedrock Knowledge Bases features

3. Cloud Control resources may take time

The project includes a time_sleep resource because of a race condition when creating:

vector bucket

vector index

IAM attachment

knowledge base

4. Ingestion uses local-exec

The ingestion job runs through the local AWS CLI, so the machine running Terraform must:

have AWS CLI installed

be authenticated

have permission to start Bedrock ingestion jobs

5. Domain/DNS option requires ownership

If using Route 53 + CloudFront, make sure the domain is actually registered and manageable from your AWS account.

6. Two API Gateway deployments exist

Your code contains both:

aws_api_gateway_deployment.deploy

aws_api_gateway_deployment.deploy2

You may eventually want to simplify this to one deployment flow to avoid confusion.

Security Considerations

This project is a good learning build, but before production use you should improve:

restrict overly broad IAM permissions such as Resource = "*"

tighten CORS from '*' to your frontend domain

add authentication to API Gateway

add WAF in front of CloudFront/API

use encrypted buckets and KMS where appropriate

add logging/monitoring/alarms

validate and sanitize user prompts before sending to the model

Future Improvements

Some strong next upgrades for this project would be:

Cognito authentication for users

DynamoDB chat history

WAF protection

custom domain for API Gateway

private subnets + NAT strategy for compute

CI/CD deployment with GitHub Actions

better observability with CloudWatch dashboards and alarms

stronger least-privilege IAM policies

support for multiple models and model routing

Useful Commands
Format Terraform

terraform fmt -recursive

Validate configuration

terraform validate

Show current state

terraform show

Destroy infrastructure

terraform destroy

What This Project Is Good For

This project is useful for learning and demonstrating:

Terraform module usage

AWS networking fundamentals

Bedrock Knowledge Base setup

RAG infrastructure design

Lambda + API Gateway integration

static site hosting with CloudFront

Route 53 and ACM integration

License

No License

Author

Gavin Forge contract me at markedsync@gmail.com
