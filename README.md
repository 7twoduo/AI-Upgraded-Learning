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

This project deploys a complete Retrieval Augmented Generation (RAG) system using Infrastructure as Code.

It automatically provisions:

Component	Description
📄 S3 Document Storage	Upload knowledge base documents
🧠 Bedrock Knowledge Base	AI retrieval system
🧬 S3 Vector Database	Vector embeddings storage
⚡ Lambda Chat Engine	Handles prompts + model responses
🌐 API Gateway	Public /chat endpoint
☁️ CloudFront CDN	Optional global frontend hosting
🔐 IAM Roles	Secure model and storage access
🌎 VPC + Subnets	Isolated infrastructure
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

🧠 System Architecture

<img width="2470" height="129" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/2068f9fa-592b-4f58-a430-ec34b8177acd" />


Hosting Options

S3 + Cloudfront
<img width="1332" height="366" alt="saase" src="https://github.com/user-attachments/assets/2332478b-a86d-47cc-b71f-f22ccf3c66de" />


    &
Ec2 Website
<img width="1652" height="129" alt="mermaid-diagram (3)" src="https://github.com/user-attachments/assets/e1166311-a0c7-4ac2-a182-750ac09a20ec" />



📂 Project Structure
.
├── main.tf
├── var.tf
├── output.tf
│
├── modules/
│   ├── vpc
│   ├── subnet
│   └── acm_certificate
│
├── RAG_Documents/
│
├── lambda_function.py
├── lambda.zip
│
└── index.html



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

🧩 RAG Pipeline

1️⃣ Upload documents
2️⃣ Terraform uploads them to S3
3️⃣ Documents are embedded using Bedrock Titan embeddings
4️⃣ Stored inside S3 Vectors
5️⃣ Queries retrieve context from vectors
6️⃣ Claude generates final response

<img width="2502" height="1206" alt="22222" src="https://github.com/user-attachments/assets/92f895f1-a6c4-494a-8a45-60eeeaed519d" />


🔐 Security Model

The infrastructure implements several AWS security practices:

✔ IAM least-privilege roles
✔ Private networking using VPC
✔ API Gateway throttling
✔ CloudFront Origin Access Control
✔ No public access to vector storage

Future improvements:

AWS WAF

Cognito authentication

request validation

encrypted secrets rotation



## Quick start

1) Pre-reqs

Terraform ~> 1.14.5  | AWS provider 6.33.0  | AWS CLI installed + authenticated  | Backend bucket already exists: backend-extra-unique-1


🎬 Animated Footer: Working Demo on Topic
     
https://github.com/user-attachments/assets/2a94670d-3471-4304-9750-13332952fb12

🎬 Animated Footer: Not Working Demo On Non Topic

https://github.com/user-attachments/assets/cae5d5fd-cb3e-4cd7-bfd2-d1a15cfc21f0

🎯 Why This Project Matters

This project demonstrates real-world cloud engineering skills:

Infrastructure as Code

AI system architecture

serverless backends

secure cloud networking

vector databases

production deployment patterns

This type of architecture is used by modern AI SaaS platforms.

🧑‍💻 Author

Built as a production-style AI infrastructure project using Terraform and AWS.

