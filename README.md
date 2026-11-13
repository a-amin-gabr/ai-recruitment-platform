# AI Recruitment Platform — Architecture & Decision Docs

**Team:** Karim Ehab (Team Lead · AI/NLP) · Abdallah Gabr (Full-Stack, Cloud & DevOps)  
**Scope:** Phase 1 Prototype — Explainable Multi-Channel CV Aggregation & Top-N Ranking  
**Stack:** Next.js + NestJS + PostgreSQL/S3/SQS (AWS `us-east-1`) · Python FastAPI + Azure OpenAI + Document Intelligence (Azure `East US 2`) · Pinecone + Langfuse

> Recruiter problem: teams receive hundreds of CVs across email, apply links, and bulk folders. This platform aggregates them in one place, extracts structured skills/experience from PDF, DOCX, and scanned resumes, and returns a deterministic, evidence-backed ranked shortlist with verbatim CV quotes at **< \$0.01 AI cost per CV**.

---

## Start Here

- **[docs/README.md](docs/README.md)** — Full document index and 20-ADR decision log
- **[docs/00-meeting-guide.md](docs/00-meeting-guide.md)** — Company pitch & decision meeting guide (led by **Karim**), time boxes, and sign-off checklist
- **[docs/01-vision-and-scope.md](docs/01-vision-and-scope.md)** — Pitch, prototype scope, and quantitative demo targets
- **[docs/02-architecture-overview.md](docs/02-architecture-overview.md)** — Multi-cloud AWS + Azure architecture, ingestion flow, and ranking flow
- **[docs/03-cost-model.md](docs/03-cost-model.md)** — Itemized pricing across Prototype (~\$45–85/mo), Pilot, and Early SaaS
- **[docs/04-roadmap.md](docs/04-roadmap.md)** — 6-week prototype delivery plan & milestone split
- **[docs/05-risks.md](docs/05-risks.md)** — Risk register and mitigations
- **[docs/06-glossary.md](docs/06-glossary.md)** — Terms & acronyms reference
- **[docs/07-future-evolution.md](docs/07-future-evolution.md)** — Evolution path to Enterprise SaaS & Phase 2 (zero-app browser 360° room scan, voice interviewer, video review)
- **[docs/08-competitor-analysis.md](docs/08-competitor-analysis.md)** — Market positioning, competitor pricing/weaknesses (18+ platforms), 6 differentiators, and feature suggestions
- **[docs/decisions/](docs/decisions/)** — Architecture Decision Records ([ADR-001](docs/decisions/001-team-and-ownership.md) through [ADR-020](docs/decisions/020-external-connectors-and-linkedin.md))

---

## Repository Status

This repository currently contains the complete architecture, cost model, competitor analysis, and 20 Architecture Decision Records (`docs/`) prepared for design sign-off before scaffolding application and Terraform code.
