
· Shared Schemas First: The grant-schemas repo defines the canonical data models (Grant, Application, Disbursement, etc.). All services consume and produce data validating against these schemas.
· API-First Design: All core services expose well-defined REST or gRPC APIs, with OpenAPI specifications.
· Event-Driven Extensibility: Significant state changes (e.g., Grant.Approved, Disbursement.Created) are published as events to an internal message bus (e.g., NATS, Kafka), allowing other services (Analytics, Compliance, Notifications) to react asynchronously.
· Centralized Identity & Audit: AuthN/AuthZ and audit logging are treated as platform services used by all other components.

```

```Component Interaction Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     OpenGrantStack Unified Architecture                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────┐    ┌──────────────┐    ┌──────────────┐               │
│  │  Mobile/Web │    │  3rd Party   │    │   Admin      │               │
│  │   Clients   │    │  Systems     │    │   Portal     │               │
│  └──────┬──────┘    └──────┬───────┘    └──────┬───────┘               │
│         │                  │                   │                        │
│         └──────────────────┼───────────────────┘                        │
│                            │                                            │
│                    (HTTPS / API Gateway)                                │
│                            │                                            │
│  ┌──────────────────────────────────────────────────────────┐           │
│  │                API Gateway & Edge Layer                  │           │
│  │  • Routing, Rate Limiting, API Key Management           │           │
│  └──────────────────────────────────────────────────────────┘           │
│                            │                                            │
│         ┌──────────────────┼──────────────────┐                         │
│         │                  │                  │                         │
│         ▼                  ▼                  ▼                         │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────┐                 │
│  │   Identity  │  │ Grant Mgmt.  │  │  Compliance    │                 │
│  │   Service   │  │   Service    │  │   Engine       │                 │
│  │ (OIDC/RBAC) │  │ (Core API)   │  │ (Rules Engine) │                 │
│  └─────────────┘  └──────────────┘  └────────────────┘                 │
│         │                  │                  │                         │
│         └──────────────────┼──────────────────┘                         │
│                            │                                            │
│                    (Internal Event Bus)                                 │
│                            │                                            │
│         ┌──────────────────┼──────────────────┐                         │
│         │                  │                  │                         │
│         ▼                  ▼                  ▼                         │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────┐                 │
│  │ Audit Ledger│  │  Analytics   │  │ Notifications  │                 │
│  │   Service   │  │   Service    │  │   Service      │                 │
│  └─────────────┘  └──────────────┘  └────────────────┘                 │
│                                                                         │
│  ┌──────────────────────────────────────────────────────────┐           │
│  │                 Shared Data & Contracts                  │           │
│  │  • grant-schemas (JSON Schemas)                         │           │
│  │  • SDKs (TypeScript, Python, etc.)                      │           │
│  └──────────────────────────────────────────────────────────┘           │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```


