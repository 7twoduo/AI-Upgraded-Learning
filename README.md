<div align="center">

# ⚔️ Bedrock RAG Chat System — Terraform + CloudFront + Edge Content Delivery

**Production-minded RAG pipeline:** S3 docs → S3 Vectors → Bedrock Knowledge Base → Lambda Chat API → (Optional) Website Hosting

<!-- Replace with your own banner image/gif -->


<br/>

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![CloudFront](https://img.shields.io/badge/CloudFront-FF9900?style=for-the-badge&logo=amazonaws&logoColor=black)
![Edge Security](https://img.shields.io/badge/Edge%20Security-0B1220?style=for-the-badge)
![Bedrock](https://img.shields.io/badge/Bedrock-00A1C9?style=for-the-badge&logo=amazonaws&logoColor=white)
![Observability](https://img.shields.io/badge/Observability-22C55E?style=for-the-badge)
![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=for-the-badge&logo=amazonaws)
![Terraform](https://img.shields.io/badge/Terraform-1.14+-purple?style=for-the-badge&logo=terraform)
![CloudFront](https://img.shields.io/badge/CloudFront-Edge%20CDN-blue?style=for-the-badge)
![Bedrock](https://img.shields.io/badge/Amazon-Bedrock-green?style=for-the-badge)
![Observability](https://img.shields.io/badge/Observability-CloudWatch-yellow?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Production%20Style-success?style=for-the-badge)
</div>

---

## What this builds (fast)

✅ Uploads local docs from `RAG_Documents/` to S3  
✅ Creates **S3 Vectors** (VectorBucket + Index)  
✅ Creates **Bedrock Knowledge Base** + **Data Source**  
✅ Triggers **ingestion job** (AWS CLI) to sync vectors  
✅ Deploys **Lambda** + **API Gateway** (`POST /chat`) with CORS  
✅ Optional website hosting:
- **Option A:** EC2 + Nginx  
- **Option B (default):** S3 + CloudFront + Route 53 + ACM

---

## Architecture (visual)

# 🚀 What This Project Builds

This repository provisions a **Retrieval Augmented Generation (RAG) system on AWS** entirely with **Terraform**.

It automatically deploys:

- 📄 **S3 document storage**
- 🧠 **Bedrock Knowledge Base**
- 🧬 **S3 Vectors vector database**
- ⚡ **Lambda chat engine**
- 🌐 **API Gateway `/chat` endpoint**
- ☁️ **Optional website hosting**

---

# 🧠 RAG Architecture

<img width="1802" height="1206" alt="mermaid-diagram (1)" src="https://github.com/user-attachments/assets/d039dd1b-d33b-4735-a3ae-b762477e3f28" />

flowchart LR

Docs[RAG Documents] --> S3[(S3 Document Bucket)]

S3 --> Vectors[S3 Vectors]
Vectors --> Index[Vector Index]

Index --> KB[Bedrock Knowledge Base]

KB --> Lambda[Lambda Chat Handler]

Lambda --> API[API Gateway /chat]

API --> Client[Website or Client App]

Hosting Options

S3 + Cloudfront
    &
Ec2 Website



📁 Repository Layout
.
├── main.tf
├── var.tf
├── output.tf
├── modules/
│
├── RAG_Documents/
│
├── lambda_function.py
├── lambda.zip
│
└── index.html

⚙️ Infrastructure Components
Component	Purpose
S3 Bucket	Stores RAG documents
S3 Vectors	Vector database for embeddings
Bedrock KB	Knowledge base retrieval
Lambda	Chat processing
API Gateway	Public /chat endpoint
CloudFront	Edge CDN for frontend
Route53	DNS management
VPC	Network isolation
IAM	Access control
🧩 Key Features

✔ Upload local documents automatically
✔ Vectorize documents using Titan embeddings
✔ Query with Claude via Bedrock
✔ Fully automated Terraform deployment
✔ Optional CloudFront production hosting

⚡ Quick Deploy
1️⃣ Initialize Terraform
terraform init
2️⃣ Plan Infrastructure
terraform plan
3️⃣ Deploy
terraform apply
🧪 Test the Chat API
curl -X POST \
https://YOUR_API_URL/chat \
-H "Content-Type: application/json" \
-d '{"message":"Explain my documents"}'
🔐 Security Notes

The project currently includes:

IAM roles for Lambda + Bedrock

API Gateway throttling

CloudFront Origin Access Control

Private subnets for internal components

Future improvements could include:

WAF

Cognito authentication

request validation

tighter IAM policies

Quick start
1) Pre-reqs

Terraform ~> 1.14.5

AWS provider 6.33.0

AWS CLI installed + authenticated

Backend bucket already exists: backend-extra-unique-1

Important notes (read this)

Backend state bucket must exist before terraform init

Bedrock model access must be enabled in your account

Ingestion runs via local-exec, so AWS CLI permissions matter

Some resources use a 30s sleep to reduce race conditions

Security upgrades (next)

If you want this “real production”:

lock down IAM (remove broad "*" resources)

restrict CORS to your domain

add auth (Cognito/JWT) to API Gateway

add WAF (CloudFront / API Gateway)

enable access logs + alarms


📊 Deployment Flow
🎯 Why This Project Matters

This project demonstrates real-world DevOps + AI infrastructure skills:

Infrastructure as Code

AI systems architecture

Bedrock integration

Serverless backend design

edge hosting

observability patterns

🧑‍💻 Author

Terraform + AWS AI infrastructure project.

Built as a production-style learning environment for RAG systems.


