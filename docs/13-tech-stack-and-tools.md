# 13 · Tech Stack & Tools Matrix

## Purpose
A single **reference catalogue** of the tools mentioned across these docs — what each is for, when to choose it, alternatives, and drawbacks — so teams make consistent tooling choices.

## When to use
- Choosing a tool for a new project
- Evaluating a replacement for an existing tool
- Onboarding: "what is this tool and why do we use it?"

## Use cases
- Building a new web app stack end-to-end
- Standardising tools across teams
- Budget planning (licensed vs open-source)

---

## User guide (non-technical)
Each row below answers: *What is it? Why do we need it? What are the alternatives? What are its downsides?* Tools marked **(OSS)** are open-source/free to self-host; others are paid SaaS or have paid tiers.

---

## Developer guide

### 1. Frontend
| Tool | Purpose | Choose when | Alternatives | Drawbacks |
|---|---|---|---|---|
| React (OSS) | UI library | Large ecosystem, flexible | Vue, Svelte, Angular, Solid | Many choices to assemble (routing, state) |
| Next.js (OSS) | React framework with SSR/SSG | SEO, full-stack React | Remix/React Router, Nuxt (Vue), SvelteKit, Astro | Complexity of rendering modes; best on Vercel |
| Angular (OSS) | Full framework | Enterprise, opinionated structure | React, Vue | Steeper learning curve, verbose |
| Vue (OSS) | Progressive UI framework | Gentle learning curve | React, Svelte | Smaller enterprise ecosystem |
| Tailwind CSS (OSS) | Utility CSS | Fast styling, design consistency | CSS Modules, styled-components, MUI | Verbose class lists |
| Vite (OSS) | Build tool/dev server | Fast dev builds | Webpack, Turbopack, Rspack | Plugin ecosystem differs from Webpack |

### 2. Backend frameworks
| Tool | Language | Choose when | Drawbacks |
|---|---|---|---|
| NestJS | TypeScript | Structured Node backends | Decorator-heavy, boilerplate |
| Express / Fastify | TypeScript/JS | Minimal APIs | Express lacks structure; DIY everything |
| FastAPI | Python | Async APIs, ML serving | Younger ecosystem than Django |
| Django | Python | Batteries-included web apps, admin | Monolithic, sync-first heritage |
| Spring Boot | Java/Kotlin | Enterprise services | Memory, startup time, "magic" |
| ASP.NET Core | C# | High-performance APIs, Microsoft stack | Microsoft ecosystem gravity |
| Gin / Echo / Fiber / Chi | Go | Fast lightweight services | Less built-in structure |
| Laravel | PHP | Rapid web apps | PHP hosting long-running constraints |
| Rails | Ruby | Rapid CRUD apps | Performance at scale |

### 3. Mobile
| Tool | Choose when | Drawbacks |
|---|---|---|
| Swift / SwiftUI | Best iOS native UX | iOS-only codebase |
| Kotlin / Jetpack Compose | Best Android native UX | Android-only codebase |
| Flutter | One codebase, custom UI | App size, Dart niche |
| React Native (Expo) | Web team going mobile | Native modules for advanced features |

### 4. Data & messaging
| Tool | Purpose | Drawbacks |
|---|---|---|
| PostgreSQL (OSS) | Primary relational DB | Horizontal write scaling needs extra tools |
| MySQL (OSS) | Relational DB | Fewer advanced features than Postgres |
| MongoDB | Document DB | Schema drift, licensing (SSPL) |
| Redis / Valkey (OSS) | Cache, sessions, queues | Memory cost; persistence trade-offs |
| Elasticsearch / OpenSearch | Search & log analytics | Resource hungry, ops-heavy |
| Kafka (OSS) | Event streaming | Operational complexity (use managed: Confluent, MSK) |
| RabbitMQ (OSS) | Message broker | Less suited for replay/streaming |
| AWS SQS/SNS | Managed queue/pub-sub | AWS lock-in; SQS ordering only in FIFO |

### 5. DevOps & delivery
| Tool | Purpose | Alternatives | Drawbacks |
|---|---|---|---|
| Git | Version control | — | Learning curve for advanced ops |
| GitHub / GitLab / Bitbucket | Code hosting + CI | Azure DevOps | See [Git Workflow](04-git-workflow.md#10-git-hosting-platforms) |
| GitHub Actions | CI/CD | GitLab CI, Jenkins, CircleCI | Minutes cost, YAML sprawl |
| Docker (OSS) | Containers | Podman | Desktop licensing for large companies |
| Kubernetes (OSS) | Container orchestration | ECS, Cloud Run, Nomad | Complexity |
| Helm (OSS) | K8s packaging | Kustomize | Templating complexity |
| Argo CD / Flux (OSS) | GitOps deploys | Spinnaker, Harness | K8s-only |
| Terraform / OpenTofu | IaC | Pulumi, CDK, Bicep | State management |
| Vault / cloud secret managers | Secrets | Doppler, 1Password | Vault ops burden |
| LaunchDarkly / Unleash / Flagsmith | Feature flags | ConfigCat, GrowthBook | Cost (LaunchDarkly), flag debt |

### 6. Quality & security
| Tool | Purpose | Drawbacks |
|---|---|---|
| ESLint / Prettier / Ruff / Black | Lint & format | Config debates |
| SonarQube / SonarCloud | Code quality + SAST | Noise; paid tiers |
| CodeQL / Semgrep | SAST | False positives |
| Snyk / Dependabot / Renovate | Dependency updates & CVEs | PR noise |
| Trivy / Grype (OSS) | Container & IaC scanning | Needs triage process |
| Gitleaks / TruffleHog (OSS) | Secret scanning | Regex false positives |
| OWASP ZAP (OSS) | DAST | Requires tuning |
| Playwright / Cypress | E2E testing | Flakiness if poorly written |
| k6 / JMeter | Load testing | Realistic data/traffic modelling effort |

### 7. Observability
| Tool | Purpose | Drawbacks |
|---|---|---|
| OpenTelemetry (OSS) | Vendor-neutral instrumentation | Still evolving for some languages |
| Prometheus + Grafana (OSS) | Metrics & dashboards | Long-term storage needs Thanos/Mimir |
| Loki / ELK | Logs | ELK resource cost |
| Jaeger / Tempo (OSS) | Tracing | Storage scaling |
| Datadog / New Relic / Dynatrace | All-in-one SaaS | Expensive at scale |
| Sentry | Error tracking | Event quota costs |
| PagerDuty / Opsgenie / incident.io | On-call & incidents | Per-user pricing |

### 8. Payments
See [Payment Integration § Tools](06-payment-integration.md#12-tools-used-across-the-payment-stack).

### 9. Cloud providers
| Provider | Strengths | Drawbacks |
|---|---|---|
| AWS | Broadest services, market leader | Complex pricing & console; many overlapping services |
| Azure | Enterprise/Microsoft integration, Entra ID | Naming/portal complexity |
| Google Cloud | Data/AI (BigQuery, Vertex), GKE, Cloud Run | Smaller enterprise footprint |
| DigitalOcean / Linode / Hetzner | Simple, cheap | Fewer managed services |
| Vercel / Netlify / Cloudflare | Frontend & edge hosting | Cost at scale; limited backend capabilities |

### 10. Collaboration & docs
| Tool | Purpose | Drawbacks |
|---|---|---|
| Jira / Linear / GitHub Projects | Work tracking | Jira complexity; Linear fewer enterprise features |
| Confluence / Notion / Markdown in repo | Documentation | Wikis drift from code — prefer docs-as-code for technical docs |
| Slack / Teams | Communication & alerts | Notification overload |
| Figma | Design & handoff | Cost per editor |
| Mermaid / draw.io / Excalidraw | Diagrams | Mermaid layout control is limited |

---

## Drawbacks of tool sprawl
| Problem | Mitigation |
|---|---|
| Too many overlapping tools | Maintain this matrix as the approved list |
| License & SaaS cost creep | Quarterly tooling review |
| Knowledge fragmentation | Golden-path templates for new projects |
| Security exposure via integrations | Review OAuth app permissions; SSO everywhere |

## Checklist
- [ ] New tool evaluated against existing alternatives in this matrix
- [ ] Cost, security, and lock-in considered
- [ ] Owner assigned for the tool
- [ ] This document updated

## Related docs
[Languages](03-languages-and-usecases.md) · [CI/CD Flow](05-ci-cd-flow.md) · [Deployment Flow](07-deployment-flow.md)
