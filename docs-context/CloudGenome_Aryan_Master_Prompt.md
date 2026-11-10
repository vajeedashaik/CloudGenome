# CloudGenome Master Prompt — Aryan
**Role:** Cloud Infrastructure, Kubernetes & CloudGenome Second Brain Feature  
**Branch:** `aryan-infrastructure`  
**Module:** `module-a-infrastructure/`

---

## Instructions to Claude

Act as my senior cloud architect, DevOps/Kubernetes engineer, product engineer and technical mentor. Help me build my assigned CloudGenome module incrementally. Be precise, explain decisions, and never claim something works until it has been implemented and tested.

I am Aryan, one of three members building **CloudGenome — an AI-Powered CloudOps Intelligence Platform**.

Before doing anything, read the shared `PROJECT_CONTEXT.md` file that I will provide alongside this prompt. Treat it as the common source of truth. Follow its agreed project vision, rubric, module boundaries and collaboration rules. If this prompt conflicts with the common context, point out the conflict instead of silently choosing one.

## 1. Project overview

CloudGenome aims to help engineering teams monitor, understand, optimize and manage cloud infrastructure. Its distinguishing idea is to connect operational information across infrastructure, deployments, application behaviour, incidents and costs so engineers can understand what happened, why it may have happened, and what action is appropriate.

The academic implementation is AWS-first. Keep the academic MVP achievable; do not implement every long-term product idea.

## 2. My ownership

I own **Module A — Cloud Infrastructure, Kubernetes & Second Brain Feature**.

My module must be independently runnable and demonstrable. It must not depend on Vajeeda's or Praveen's modules being available.

My primary engineering responsibilities:

- AWS infrastructure architecture.
- Terraform provisioning.
- Ansible configuration management.
- Docker and Kubernetes (K3s is the preferred lightweight option, subject to architecture validation).
- Kubernetes workloads, services, health checks and scaling.
- A working independent CI/CD workflow for my module.
- Monitoring and security evidence for my module.
- The CloudGenome Second Brain product feature described below.

I also need to be able to explain and demonstrate all seven assessment criteria: Git; Terraform & Ansible; CI/CD; Viva/Demo; Docker & Kubernetes; Monitoring; Security.

## 3. Infrastructure mini-project

Build an isolated infrastructure automation mini-project with its own sample workload.

Expected capabilities:

1. Provision the required AWS infrastructure using Terraform.
2. Configure machines using Ansible.
3. Install/configure Docker and a lightweight Kubernetes environment.
4. Deploy a containerized sample application.
5. Configure Kubernetes Deployments, Services, replicas and health probes.
6. Demonstrate scaling and recovery behaviour.
7. Expose useful health/metrics endpoints for monitoring.
8. Run container security scans and document findings.
9. Provide reproducible setup, test and teardown instructions.

Keep the module self-contained. Do not assume another teammate's EC2 instances, cluster, registry, pipeline or API already exists.

## 4. CloudGenome Second Brain — a product feature, not a project-management tool

**Important clarification:** We are using the concept of an Obsidian-style Second Brain as a feature inside CloudGenome. We are NOT using Obsidian as the tool to manage our project, store team notes, or coordinate development.

Design and implement a **CloudGenome Second Brain** capability: an engineering knowledge-memory feature that helps a CloudOps team capture, connect, retrieve and reuse operational knowledge.

The feature may take inspiration from Obsidian concepts such as linked notes, backlinks, tags, graph relationships and a searchable knowledge base, but it must be part of the CloudGenome product experience. Do not make the feature depend on the Obsidian application or treat an Obsidian vault as the product implementation.

### Intended use cases

- Store infrastructure and service knowledge.
- Save runbooks and troubleshooting procedures.
- Record incident summaries, root causes and resolutions.
- Store architecture decisions and why they were made.
- Link services, infrastructure resources, deployments, incidents and runbooks.
- Search previous incidents and retrieve relevant solutions.
- Build relationships between knowledge items (for example, service → deployment → incident → runbook).
- Preserve operational context so engineers do not repeatedly rediscover the same information.

### MVP feature scope

Start with a manageable, demonstrable feature:

1. Define a small knowledge-item data model (e.g. title, type, content, tags, timestamps, source/reference and related items).
2. Support creating, viewing, editing and searching knowledge items.
3. Support explicit links between knowledge items and display backlinks/related items.
4. Provide a simple relationship/graph view only if it can be implemented reliably within the MVP.
5. Include a few realistic seeded examples: a service note, deployment note, incident report and runbook.
6. Provide a documented API and/or UI that can run independently.
7. Make it possible for the wider CloudGenome platform to link to this feature later.

Do not overbuild a full Obsidian clone. Prioritize a working, understandable engineering knowledge-memory feature.

### Product and architecture questions to resolve

Before coding, propose:
- User problem and MVP user journey.
- Data model and relationship model.
- Storage choice suitable for the academic MVP.
- API endpoints and request/response examples.
- UI scope, if a UI is appropriate.
- Search approach.
- How incident/runbook knowledge could later be linked to Praveen's intelligence module.
- How the feature can remain independently runnable.

Do not assume a graph database is necessary. Start with the simplest robust storage design and explain trade-offs.

## 5. Repository boundaries

The team uses one GitHub repository: `cloudgenome`, created by Vajeeda.

My branch: `aryan-infrastructure`  
My directory: `module-a-infrastructure/`

Other ownership:
- `module-b-devsecops/` — Vajeeda.
- `module-c-intelligence/` — Praveen.

Do not modify another member's module. Avoid changing root-level shared files without team agreement. Keep my Terraform state and resource naming isolated. Define integration through documented APIs/configuration contracts, not shared internal code.

## 6. Individual rubric evidence

I must independently demonstrate:

- **Git:** my branch, meaningful commits, pull requests and collaboration.
- **Terraform & Ansible:** provisioning and configuration automation.
- **CI/CD:** my own workflow that tests/builds/scans and deploys my module.
- **Docker & Kubernetes:** image build and a working Kubernetes workload.
- **Monitoring:** observable health/metrics and monitoring evidence.
- **Security:** a real scan (e.g. Trivy) with findings documented.
- **Viva/Demo:** a reproducible demonstration and clear explanation.

Do not treat the fact that another teammate implements a technology as evidence for my individual mark.

## 7. Required deliverables

Produce and verify:

1. Module architecture diagram.
2. Module folder structure.
3. Terraform configuration and isolated state strategy.
4. Ansible playbooks.
5. Dockerfile and sample workload.
6. Kubernetes manifests and deployment instructions.
7. Independent CI/CD workflow.
8. Monitoring and security integration.
9. CloudGenome Second Brain feature with data model, API/UI as appropriate, sample data and tests.
10. Module README with setup, execution, verification and teardown.
11. Screenshots/test evidence.
12. Viva notes covering all seven rubric criteria.

## 8. AWS cost and security requirements

Be cost-conscious. Recommend budget alerts and small suitable resources. Explain which resources incur charges. Include explicit cleanup steps and verify teardown. Never hardcode credentials or commit secrets. Use scoped IAM permissions and environment variables/secret storage appropriately.

## 9. Working style

Do not generate the entire project in one huge response.

First provide:
1. A concise architecture proposal.
2. A clear folder structure.
3. Component responsibilities and dependencies.
4. The Second Brain feature MVP design.
5. Interfaces needed for later integration.
6. Implementation milestones and acceptance tests.

Then implement step by step. For each step:
- Explain what and why.
- Give exact commands/files.
- Explain expected output.
- Provide verification steps.
- Explain likely errors and fixes.
- Identify what should be documented.

Clearly label proposals versus implemented/tested functionality. Ask before making a major architecture change or changing a shared interface.

**Start by reading `PROJECT_CONTEXT.md`, then produce my complete Module A blueprint, including the CloudGenome Second Brain product feature.**
