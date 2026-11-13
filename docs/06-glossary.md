# 06. Glossary

| Term | Meaning |
|---|---|
| **ADR** | Architecture Decision Record: a short document recording one decision, its options and why it was chosen |
| **ALB** | Application Load Balancer (AWS): routes HTTP traffic to containers |
| **ACA** | Azure Container Apps: serverless container hosting on Azure that can scale to zero |
| **ATS** | Applicant Tracking System (Workday, Greenhouse, SAP SuccessFactors…) |
| **BM25** | Classic keyword-ranking algorithm used by search engines |
| **Blind scoring** | Removing name, gender, age, photo etc. before AI scoring to reduce bias |
| **Cosine similarity** | How close two embedding vectors are (1 = same meaning) |
| **Dense vector / embedding** | List of numbers that represents the *meaning* of text |
| **Sparse vector** | Keyword-based vector; most values are zero (lexical matching) |
| **Hybrid search** | Combining dense (semantic) and sparse (keyword) search |
| **RRF** | Reciprocal Rank Fusion: simple, robust way to merge two ranked lists |
| **Reranker / cross-encoder** | Model that rescores a short list more accurately than the first-stage search |
| **DLQ** | Dead-Letter Queue: where failed messages go after N retries |
| **DSAR** | Data Subject Access Request: a person's legal right to see or delete their data |
| **ECS Fargate** | AWS serverless containers (no servers to manage) |
| **Entra ID** | Microsoft's identity service (formerly Azure AD) |
| **EU AI Act** | EU law; AI used for hiring is classified as **high-risk** (needs transparency, human oversight, logging) |
| **GDPR / PDPL** | EU data-protection law / Egypt Personal Data Protection Law (Law 151/2020) |
| **IaC** | Infrastructure as Code (Terraform): cloud resources defined in files |
| **JD** | Job Description |
| **LCU** | Load Balancer Capacity Unit: ALB usage-based charge |
| **Managed Identity** | Azure feature letting a service authenticate to other Azure services without keys |
| **Matryoshka embeddings** | Embeddings that can be truncated (e.g. 3072 → 1024 dims) with small quality loss |
| **MAU** | Monthly Active Users |
| **MRR** | Mean Reciprocal Rank: how high the first relevant result appears |
| **Namespace (Pinecone)** | Partition inside an index; we use one per tenant (customer company) |
| **NDCG@10** | Normalised Discounted Cumulative Gain at 10: standard metric for "is the Top 10 in the right order" (0–1, higher is better) |
| **OIDC** | OpenID Connect: lets GitHub Actions get short-lived cloud credentials (no stored keys) |
| **P@10** | Precision at 10: share of the Top 10 that are relevant |
| **PII** | Personally Identifiable Information |
| **Presigned URL** | Temporary URL granting access to one S3 object for a few minutes |
| **RU / WU** | Pinecone Read Units / Write Units (billing units) |
| **Structured Outputs** | Azure OpenAI feature forcing the model to return JSON that matches a schema |
| **Tenant** | One customer company in a multi-customer (SaaS) system |
| **TPM** | Tokens Per Minute: Azure OpenAI rate limit/quota |
| **Token** | Piece of text (~¾ of an English word) used for LLM billing |
