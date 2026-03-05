<div align="center">

# ⚔️ Bedrock RAG Chat System — Terraform + CloudFront + Edge Security

**Production-minded RAG pipeline:** S3 docs → S3 Vectors → Bedrock Knowledge Base → Lambda Chat API → (Optional) Website Hosting

<!-- Replace with your own banner image/gif -->
<img src="YOUR_BANNER_IMAGE_OR_GIF_HERE" width="900" />

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

### RAG Flow
```mermaid
flowchart LR
  A[RAG_Documents/] -->|Terraform uploads| B[(S3 Docs Bucket)]
  B --> C[S3 Vectors VectorBucket]
  C --> D[S3 Vectors Index]
  D --> E[Bedrock Knowledge Base]
  E --> F[Lambda Chat Handler]
  F --> G[API Gateway /chat]
  G --> H[Frontend]

Hosting Options

S3 + Cloudfront
    &
Ec2 Website



Repo layout
.
├── main.tf
├── var.tf
├── output.tf
├── modules/
├── RAG_Documents/
├── index.html
├── lambda_function.py
└── lambda.zip

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





