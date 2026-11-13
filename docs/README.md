# Prototype Decision Docs: AI CV Aggregation & Ranking

**Team:** Karim Ehab (AI/NLP lead) · Abdallah Gabr (Full-Stack, Cloud & DevOps)
**Scope:** Phase 1 prototype. Collect CVs from several channels, rank them, and show *why* each candidate got their score.
**Status:** 🟡 Draft for the Abdallah + Karim decision meeting (2026-11-13)

> Prices are public list prices for **AWS `us-east-1`**, **Azure `East US 2`** and SaaS vendors, collected **Nov 2026**. Treat them as estimates. Check them again in the [AWS Pricing Calculator](https://calculator.aws/) and [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) before quoting anything to the company.

---

## How to use this folder in the meeting

1. Open [00-meeting-guide.md](00-meeting-guide.md) and follow the agenda.
2. For each decision, read the **Recommendation** and **Trade-offs**, then fill in the **🗳️ Meeting Decision** box at the bottom of the file.
3. Update the status column below (🟡 Proposed → ✅ Accepted / 🔁 Changed / ⏸️ Deferred).

## Documents

| File | What it covers |
|---|---|
| [00-meeting-guide.md](00-meeting-guide.md) | Agenda, time boxes, questions to answer, final checklist |
| [01-vision-and-scope.md](01-vision-and-scope.md) | What the prototype must prove, what is in/out of scope, success criteria |
| [02-architecture-overview.md](02-architecture-overview.md) | Full multi-cloud architecture, data flows, responsibility boundaries |
| [03-cost-model.md](03-cost-model.md) | Every price in one place + 3 scenarios (Prototype / Pilot / Early SaaS) |
| [04-roadmap.md](04-roadmap.md) | 6-week plan, who does what, demo milestones |
| [05-risks.md](05-risks.md) | Risk register and mitigations |
| [06-glossary.md](06-glossary.md) | Every acronym and term used in these docs |
| [07-future-evolution.md](07-future-evolution.md) | How the prototype grows into Pilot, Enterprise SaaS & Phase 2 without rewrites |
| [08-competitor-analysis.md](08-competitor-analysis.md) | Deep study of 18+ competitors, pricing, weaknesses, our edge & feature suggestions |
| [decisions/](decisions/) | Architecture Decision Records (ADRs): one file per decision |
| [templates/adr-template.md](templates/adr-template.md) | Template for future decisions |

## Decision Log

| ADR | Decision | Recommended option | Rough monthly cost (prototype) | Status |
|---|---|---|---|---|
| [001](decisions/001-team-and-ownership.md) | Team ownership split | Abdallah = AWS app + DevOps · Karim = Azure AI | — | 🟡 Proposed |
| [002](decisions/002-cloud-strategy.md) | Cloud strategy | AWS app layer + Azure AI layer, all in Northern Virginia | — | 🟡 Proposed |
| [003](decisions/003-frontend-framework.md) | Frontend framework | Next.js + React + Tailwind + shadcn/ui | \$0 | 🟡 Proposed |
| [004](decisions/004-backend-framework.md) | Backend framework | NestJS (TypeScript) + Prisma | \$0 | 🟡 Proposed |
| [005](decisions/005-ai-service-runtime-and-hosting.md) | AI service runtime & hosting | Python FastAPI on Azure Container Apps | \$0–5 | 🟡 Proposed |
| [006](decisions/006-llm-provider-and-model.md) | LLM provider & model | Azure OpenAI, *mini* tier (GPT-4.1-mini / GPT-5-mini) | \$3–8 | 🟡 Proposed |
| [007](decisions/007-embedding-model.md) | Embedding model | `text-embedding-3-large` @ 1024 dims | < \$1 | 🟡 Proposed |
| [008](decisions/008-cv-parsing.md) | CV parsing | Local parsers + Azure Document Intelligence Layout (S0) fallback | \$0–20 | 🟡 Proposed |
| [009](decisions/009-vector-database.md) | Vector database | Pinecone Starter (free) behind an interface; pgvector fallback | \$0 | 🟡 Proposed |
| [010](decisions/010-primary-database.md) | Primary database | PostgreSQL 16 on AWS RDS | ~\$14 | 🟡 Proposed |
| [011](decisions/011-ranking-and-scoring-method.md) | Ranking method | Hybrid recall → deterministic weighted scoring → LLM evidence check | (in 006) | 🟡 Proposed |
| [012](decisions/012-aws-compute-and-hosting.md) | AWS compute & hosting | Amplify Hosting (web) + ECS Fargate (API/worker) | ~\$30–45 | 🟡 Proposed |
| [013](decisions/013-authentication.md) | Authentication | Amazon Cognito | \$0 | 🟡 Proposed |
| [014](decisions/014-ingestion-channels-and-queue.md) | Ingestion channels & queue | Apply link + bulk ZIP + email (SES) → S3 → SQS | ~\$1 | 🟡 Proposed |
| [015](decisions/015-cross-cloud-security-and-networking.md) | Cross-cloud security | OAuth2 (Entra ID) + presigned URLs + Managed Identity | \$0 | 🟡 Proposed |
| [016](decisions/016-iac-cicd-and-repo.md) | IaC, CI/CD, repo | Terraform + GitHub Actions (OIDC) + Turborepo/pnpm + uv | \$0 | 🟡 Proposed |
| [017](decisions/017-observability-and-llm-ops.md) | Observability & LLMOps | Langfuse + Sentry + CloudWatch/Azure Monitor | \$0–5 | 🟡 Proposed |
| [018](decisions/018-evaluation-strategy.md) | Evaluation strategy | Labelled dataset + NDCG@10 vs baselines + fairness test | \$0–10 | 🟡 Proposed |
| [019](decisions/019-privacy-compliance-and-fairness.md) | Privacy, compliance, fairness | Blind scoring, audit log, retention, human-in-the-loop | \$0 | 🟡 Proposed |
| [020](decisions/020-external-connectors-and-linkedin.md) | External connectors & LinkedIn | LinkedIn email-intake + bulk ZIP now; Unified ATS / Apply Connect later | \$0–1 | 🟡 Proposed |

**Estimated prototype total: about \$45–85 / month**, and about **\$0.005–0.01 AI cost per CV**. Details in [03-cost-model.md](03-cost-model.md).
