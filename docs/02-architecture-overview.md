# 02. Architecture Overview

## 1. Big picture

```mermaid
flowchart LR
    subgraph Users
        Rec["Recruiter"]
        Cand["Candidate"]
        Mail["Email forwarding"]
    end

    subgraph AWS["AWS us-east-1: Application Layer (Abdallah)"]
        Web["Next.js on Amplify Hosting"]
        Cognito["Cognito"]
        API["NestJS API on ECS Fargate"]
        Worker["Ingestion Worker on ECS Fargate"]
        S3[("S3: raw CVs")]
        SQS[["SQS + DLQ"]]
        RDS[("PostgreSQL 16 on RDS")]
        SES["SES inbound email"]
    end

    subgraph Azure["Azure East US 2: AI Layer (Karim)"]
        AI["FastAPI AI Service on Container Apps"]
        AOAI["Azure OpenAI"]
        DI["Document Intelligence"]
        KV["Key Vault"]
    end

    subgraph SaaS["SaaS, AWS us-east-1"]
        PC[("Pinecone")]
        LF["Langfuse"]
    end

    Rec --> Web --> API
    Cand --> Web
    Rec -.-> Cognito
    Mail --> SES --> S3
    API --> S3
    S3 --> SQS --> Worker
    API <--> RDS
    Worker <--> RDS
    Worker -->|"HTTPS + OAuth2"| AI
    API -->|"HTTPS + OAuth2"| AI
    AI -->|"Managed Identity"| AOAI
    AI -->|"Managed Identity"| DI
    AI -->|"presigned URL"| S3
    AI <--> PC
    AI -.-> LF
    AI --- KV
```

## 2. The one rule that makes multi-cloud manageable

| | AWS side (NestJS) | Azure side (FastAPI) |
|---|---|---|
| Owns | Tenants, users, jobs, candidates, files, ranking history, shortlists, audit log | All AI logic: parse, extract, embed, retrieve, score, explain |
| State | **Stateful**: PostgreSQL + S3 | **Stateless**: only writes to Pinecone (a derived, rebuildable index) |
| Talks to the other side | Calls the AI service over a versioned REST contract (`/v1/...`) | Never connects to Postgres; gets files via 5-minute presigned URLs |

**Why it matters:** either side can move to the other cloud by changing adapters and Terraform only. If Pinecone is lost, re-run indexing from Postgres + S3.

## 3. CV ingestion flow

```mermaid
sequenceDiagram
    autonumber
    participant C as Channel
    participant API as NestJS API
    participant S3 as S3
    participant Q as SQS
    participant W as Worker
    participant AI as AI Service
    participant PC as Pinecone
    participant DB as PostgreSQL

    C->>API: Upload CV (apply link / ZIP / email)
    API->>DB: Insert document (RECEIVED, sha256)
    API->>S3: Store raw file
    S3-->>Q: ObjectCreated event
    Q->>W: Message with document id
    W->>AI: POST /v1/cv/process (presigned URL)
    AI->>AI: Parse, extract profile, normalise skills, embed
    AI->>PC: Upsert vectors (namespace = tenant)
    AI-->>W: CandidateProfile JSON
    W->>DB: Save profile (INDEXED)
```

## 4. Ranking flow

```mermaid
sequenceDiagram
    autonumber
    participant R as Recruiter
    participant API as NestJS API
    participant AI as AI Service
    participant PC as Pinecone
    participant LLM as Azure OpenAI
    participant DB as PostgreSQL

    R->>API: Rank candidates for job (filters + weights)
    API->>AI: POST /v1/rank (requirements, filters, weights)
    AI->>LLM: Embed job requirements
    AI->>PC: Filtered hybrid query (Top 200)
    AI->>AI: Deterministic feature scoring
    AI->>LLM: Evidence check for Top 30
    AI-->>API: Ranked list + breakdown + evidence
    API->>DB: Save ranking_run (audit, reproducible)
    API-->>R: Explainable ranked table
```

## 5. Planned monorepo layout

```text
grad/
├── apps/
│   ├── web/          # Next.js (Abdallah)
│   ├── api/          # NestJS API + ingestion worker (Abdallah)
│   └── ai-service/   # Python FastAPI (Karim)
├── packages/
│   └── shared/       # Shared TS types, Zod schemas, generated OpenAPI client
├── eval/             # Datasets, labels, evaluation scripts (Karim)
├── infra/
│   ├── terraform/aws/
│   ├── terraform/azure/
│   └── docker/
├── docs/             # These documents
└── docker-compose.yml  # Local Postgres, MinIO (S3), LocalStack SQS
```

## 6. Environments

| Environment | Where | Purpose |
|---|---|---|
| `local` | Laptop: `docker compose` (Postgres, MinIO, LocalStack) + real Azure OpenAI / Pinecone `dev` namespace | Daily development |
| `demo` | AWS + Azure (Terraform) | What the company sees; seeded demo tenant |
| `prod` | Later, after funding | — |

> One cloud environment (`demo`) is enough for the prototype. A separate `staging` doubles infra cost with little benefit for 2 people.
