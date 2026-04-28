# Tennis Intelligence Platform

> Real-time tennis analytics powered by event-driven micro-services, Apache Kafka, and cloud-native infrastructure on Azure or (multi-cloud)

---

## table of contents

- [Context](#the-problem)
- [Architecture](#architecture-overview)
  - [Level I](#level-i-c4)
  - [Level II](#level-ii-c4)
- [Micro-services](#services)
- [Stack](#tech-stack)
- [Distinguish](#what-makes-this-different)
- [Expectation](#roadmap)
- [Me](#author)

---

## The problem

During a live tennis match, broadcast data is limited to basic scorelines and first-serve percentages. The deeper intelligence — how a player performs under pressure, whether momentum is shifting, how historical patterns predict current behavior — is scattered, unprocessed, and unavailable in real time.

Tennis Intelligence Platform solves this by processing every point as an event, cross-referencing live data with historical patterns, and surfacing insights that don't exist anywhere else.

---

## Architecture overview

### Level I (C4)

![Diagram architecture C4 level 1](../statics/frame/lvl-1-v1.0.svg)

### Level II (C4)

- [ ] TODO: Design Level II (C4) platform architecture

---

## Services

| Repository                                                                       | Language  | Responsibility                                                            |
| -------------------------------------------------------------------------------- | --------- | ------------------------------------------------------------------------- |
| [tip-api-gateway](https://github.com/tip-platform/tip-api-gateway)               | Go        | Single entry point. REST out, gRPC in, rate limiting, auth                |
| [tip-match-ingestion](https://github.com/tip-platform/tip-match-ingestion)       | Go        | Consumes live tennis API, publishes events to Kafka                       |
| [tip-stats-processor](https://github.com/tip-platform/tip-stats-processor)       | Go        | Kafka consumer, calculates real-time stats, writes to PostgreSQL + Valkey |
| [tip-momentum-detector](https://github.com/tip-platform/tip-momentum-detector)   | Go        | Detects momentum shifts by crossing live data with historical patterns    |
| [tip-pressure-analyzer](https://github.com/tip-platform/tip-pressure-analyzer)   | Go        | Analyzes player behavior on decisive points: tie-breaks, break points     |
| [tip-historical-scraper](https://github.com/tip-platform/tip-historical-scraper) | Python    | Scheduled scraper for ATP/WTA historical data                             |
| [tip-infrastructure](https://github.com/tip-platform/tip-infrastructure)         | Terraform | All Azure infrastructure as code                                          |

---

## Tech Stack

**Languages**

- Go (net/http standard library) — all backend services
- Python — historical data scraper

**Communication**

- REST + WebSockets — external API (clients → gateway)
- gRPC — internal service-to-service communication
- Apache Kafka — async event streaming (via Azure Event Hubs)

**Data**

- MongoDB — raw schemaless events
- PostgreSQL — processed stats via materialized views
- SQL Server — relational data (players, tournaments, rankings)
- Valkey — live match state and low-latency cache

**Reliability**

- Circuit Breaker pattern on all service calls
- Health checks and readiness probes per service
- Distributed tracing with OpenTelemetry

**Infrastructure**

- Azure Container Apps — one per microservice
- Azure Container Registry — Docker image storage
- Azure Event Hubs — Kafka-compatible managed streaming
- Azure Key Vault — secrets management
- Terraform — all infrastructure as code

**CI/CD & Observability**

- GitHub Actions — per-repository pipelines
- Azure Pipelines — Microsoft ecosystem integration
- Trivy + SonarQube + OWASP — security integrated in pipeline
- Azure Monitor + Application Insights + Grafana

---

## What Makes This Different

Most tennis data platforms show you what happened. This platform tells you **why it's happening and what it means**.

- **Momentum detection** — identifies when a player enters a positive streak and how long it historically lasts
- **Pressure performance** — measures each player's conversion rate on break points, tie-breaks, and match points compared to their historical average
- **Pattern cross-referencing** — every live insight is validated against years of historical data, not just the current match

---

## Roadmap

- [x] Architecture design and repository setup
- [ ] Phase 1 — Core services with Docker Compose locally
- [ ] Phase 2 — CI/CD pipelines and Azure deployment
- [ ] Phase 3 — Security integration (Trivy, SonarQube, OWASP)
- [ ] Phase 4 — Observability and alerting
- [ ] Phase 5 — Infrastructure as code with Terraform
- [ ] Future — LLM-powered insights engine and React dashboard

---

## Author

**Jaume Suarez** — Systems Engineer focused on cloud infrastructure and DevOps
[LinkedIn](https://linkedin.com/in/jaumesuarez) · Bogotá, Colombia
