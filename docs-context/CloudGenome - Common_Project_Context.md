# CloudGenome --- Common Project Context

**Document type:** Shared project source of truth\
**Version:** 1.0\
**Last updated:** 10 November 2026\
**Team:** Aryan, Vajeeda, Praveen

------------------------------------------------------------------------

## 1. Purpose

This document gives all three team members one consistent understanding
of CloudGenome. Share it with Claude or another assistant whenever
asking for project-specific help.

Use it for architecture, implementation planning, module boundaries,
GitHub collaboration, integration, documentation, testing and viva
preparation.

**Important:** A proposed feature is not an implemented feature. Update
this document when the team formally changes a decision.

## 2. Project identity

### CloudGenome --- AI-Powered CloudOps Intelligence Platform

CloudGenome is intended to help engineering teams monitor, understand,
optimize and manage cloud infrastructure.

Traditional monitoring tools show metrics, logs and alerts. CloudGenome
aims to connect operational information across infrastructure,
deployments, application behaviour and cloud costs to help answer:

1.  What happened?
2.  Why might it have happened?
3.  What evidence supports that explanation?
4.  What action should be considered?
5.  Did the action resolve the issue?

**Core idea:** Move beyond showing what happened to helping engineers
understand why it happened and what they should do next.

CloudGenome is not intended to be merely a dashboard, chatbot, Terraform
wrapper, Kubernetes clone or generic CI/CD tool. Its differentiating
direction is cross-system operational reasoning.

### Example

A new deployment is followed by higher CPU usage and increased cloud
costs. CloudGenome should bring together deployment history, application
and infrastructure metrics, relevant logs and alerts, and available cost
information. It can then explain the observed change, identify a
possible relationship to the deployment, and recommend a safe next step.

Any infrastructure-changing action must be controlled by policy and
approval, followed by verification.

## 3. Academic MVP versus long-term vision

These scopes must not be confused.

### Academic implementation

The immediate deliverable is a working, AWS-first CloudOps/DevOps
implementation that demonstrates the assessment rubric and integrates
three independently developed modules.

It should demonstrate: - Git and collaborative version control. -
Terraform infrastructure provisioning. - Ansible machine
configuration. - CI/CD automation. - Docker containerization. -
Kubernetes orchestration. - Monitoring and observability. - Security
scanning. - Basic CloudGenome incident and cost intelligence. - A
working end-to-end demonstration.

### Long-term product vision

Potential future capabilities: - Multi-cloud support across AWS, Azure
and GCP. - Advanced FinOps and predictive cost analysis. - AI/LLM API
usage and token-cost intelligence. - GPU workload and inference-cost
intelligence. - Kubernetes and application observability at scale. - A
knowledge graph connecting code, deployments, workloads and costs. -
Advanced AIOps, LLMOps and MLOps. - GreenOps/carbon insights. -
Security, governance and compliance intelligence. - Controlled automated
remediation and infrastructure time-travel.

These are future directions unless explicitly implemented and tested in
the academic MVP.

## 4. Assessment rubric

Each item is worth one mark. Assessment is individual, even though this
is a group project.

  -----------------------------------------------------------------------------------
  Assessment area                              Marks Evidence expected
  --------------------- ---------------------------- --------------------------------
  Git                                              1 Individual branch, meaningful
                                                     commits, collaboration and pull
                                                     requests

  Terraform & Ansible                              1 Infrastructure as code and
                                                     configuration automation

  CI/CD Pipeline                                   1 Automated
                                                     test/build/security/deployment
                                                     workflow

  Viva/Demo                                        1 Clear explanation and successful
                                                     demonstration

  Docker & Kubernetes                              1 Container image and working
                                                     Kubernetes deployment

  Monitoring Tools                                 1 Metrics, dashboards, alerts or
                                                     monitoring evidence

  Security Tools                                   1 Security scanning and documented
                                                     findings

  **Total**                                    **7** 
  -----------------------------------------------------------------------------------

**Individual-marking requirement:** Every member must be able to explain
and demonstrate all seven assessment areas. Each member's mini-project
must therefore include reproducible evidence for the rubric.

The modules can have different primary areas of ownership, but no member
should be left with only documentation, coordination or a fragment that
cannot be demonstrated independently.

## 5. Team and ownership

  -----------------------------------------------------------------------
  Member                  Primary ownership       Module
  ----------------------- ----------------------- -----------------------
  Aryan                   Cloud infrastructure,   Module A
                          Kubernetes and shared   
                          Obsidian Second Brain   

  Vajeeda                 Team leader, GitHub     Module B
                          repository              
                          administration, CI/CD   
                          and DevSecOps           

  Praveen                 Monitoring,             Module C
                          observability and       
                          CloudGenome             
                          Intelligence            
  -----------------------------------------------------------------------

### Aryan --- Module A: Infrastructure & Kubernetes

Primary focus: - AWS infrastructure architecture. - Terraform
provisioning. - Ansible configuration. - Docker and K3s/Kubernetes. -
Kubernetes workloads, services, health checks and scaling. -
Infrastructure monitoring and security evidence. - Obsidian Second Brain
setup and maintenance.

### Vajeeda --- Module B: CI/CD & DevSecOps

Primary focus: - Create the central GitHub repository. - Invite Aryan
and Praveen as collaborators. - Establish branch and pull-request
conventions. - GitHub Actions pipeline. - Automated tests and builds. -
Docker image build and Amazon ECR integration. - Trivy and
dependency/source scanning. - Deployment workflow and rollback
demonstration.

### Praveen --- Module C: Monitoring & Intelligence

Primary focus: - Prometheus and Grafana. - AWS CloudWatch integration. -
Metrics, dashboards and alert rules. - Python/FastAPI intelligence
service. - Basic incident analysis and cost analysis. - Evidence-based
AI explanations and recommendations. - Docker/Kubernetes deployment,
CI/CD, infrastructure and security evidence for the independent module.

### Shared responsibilities

All members must: - Understand the overall CloudGenome architecture. -
Maintain their own implementation and contribution evidence. - Write and
review documentation for their module. - Participate in integration and
end-to-end testing. - Prepare for all seven viva criteria. - Help
diagnose integration issues without taking ownership of another member's
module.

Aryan coordinates the Obsidian knowledge base, but all members
contribute implementation notes, decisions, commands, troubleshooting
and viva material.

## 6. Repository and collaboration model

Vajeeda creates and administers one shared GitHub repository named:

`cloudgenome`

Proposed repository structure:

``` text
cloudgenome/
├── module-a-infrastructure/       # Aryan
├── module-b-devsecops/             # Vajeeda
├── module-c-intelligence/          # Praveen
├── integration/                    # Shared integration assets
├── docs/                            # Shared project documentation
├── README.md
└── .gitignore
```

Proposed branches:

``` text
main
aryan-infrastructure
vajeeda-devsecops
praveen-intelligence
```

### Collaboration rules

1.  Each member works primarily on their own branch.
2.  Each member owns their module directory.
3.  Do not modify another member's module without agreement.
4.  Do not commit directly to `main`; use pull requests.
5.  Keep commits meaningful and attributable to the person doing the
    work.
6.  Review changes before merging.
7.  Coordinate changes to shared files such as root README, integration
    contracts and common configuration.
8.  Keep credentials, private keys, AWS access keys, tokens and secret
    `.env` files out of Git.
9.  Use `.gitignore`, GitHub Actions secrets and appropriately scoped
    IAM permissions.
10. Keep each module runnable without requiring the other two modules to
    be running.

A branch is a collaboration boundary, not a substitute for a clean
module boundary.

## 7. Independence and integration principles

Each module must be a **fully functional mini-project in itself**. It
should have: - Its own README. - Setup and execution instructions. - Its
own application/service or demonstrable workload. - Docker
configuration. - Kubernetes manifests where applicable. - CI/CD
workflow. - Terraform and Ansible demonstration. - Monitoring and
security evidence. - Test and demo procedure. - Teardown instructions.

### Module isolation

-   Module A must not require Module B or C to provision infrastructure
    and demonstrate its Kubernetes workload.
-   Module B must not require Module A or C to run its CI/CD and
    security pipeline.
-   Module C must not require Module A or B to run its monitoring and
    intelligence service.

### Integration

Connect modules through documented, stable interfaces rather than
importing or editing one another's internal code.

Agree on shared conventions before integration: - Service names and
ports. - Health-check endpoints. - Container image naming and tagging. -
Environment-variable names. - JSON API request/response formats. -
Kubernetes namespace and label conventions. - Registry and deployment
configuration. - Monitoring scrape conventions.

The integrated CloudGenome should demonstrate how the modules work
together, but each module must remain independently demonstrable.

### Cost and duplication

Individual assessment requires each member to demonstrate the rubric; it
does **not** require three permanently running, expensive AWS
environments.

Use local development, lightweight K3s clusters, isolated configurations
and temporary AWS resources where practical. Coordinate AWS account
usage carefully. Keep Terraform state isolated per module/environment.
Never share or overwrite another member's Terraform state.

## 8. Proposed overall architecture

``` text
                         GitHub Repository
                                 |
                 +---------------+---------------+
                 |               |               |
             Module A         Module B         Module C
           Infrastructure     DevSecOps       Intelligence
                 |               |               |
             Terraform       GitHub Actions   Prometheus
              Ansible         Docker Build      Grafana
              Docker          Trivy Scan       CloudWatch
              K3s/K8s          Amazon ECR       FastAPI
                 |               |               |
                 +---------------+---------------+
                                 |
                       Integration Layer
                                 |
                       CloudGenome Platform
                                 |
                 Evidence-based Analysis & Reports
                                 |
                     Recommendations / Approval
                                 |
                       Verification of Results
```

This is a logical architecture, not a claim that all connections have
already been implemented.

-   **Module A:** Provisions and configures an isolated environment,
    then deploys and operates a containerized Kubernetes workload.
-   **Module B:** Automates code validation, testing, security checks,
    image creation, registry publishing and deployment workflow.
-   **Module C:** Collects operational data, presents metrics and
    dashboards, identifies basic anomalies/incidents, and generates
    evidence-based explanations and recommendations.

## 9. Proposed technology stack

  -----------------------------------------------------------------------
  Area                                Proposed technology
  ----------------------------------- -----------------------------------
  Cloud                               AWS-first

  Version control                     Git, GitHub

  Infrastructure as Code              Terraform

  Configuration management            Ansible

  Containers                          Docker

  Orchestration                       K3s / Kubernetes

  CI/CD                               GitHub Actions

  Container registry                  Amazon ECR

  Security scanning                   Trivy, dependency/source scanning

  Monitoring                          Prometheus, Grafana, AWS CloudWatch

  Intelligence API                    Python, FastAPI

  AI reasoning                        Optional LLM integration after
                                      deterministic analysis works

  Documentation                       Obsidian, Markdown

  Collaboration                       GitHub branches, pull requests and
                                      issues
  -----------------------------------------------------------------------

Technology choices may be adjusted after architecture validation. Avoid
adding tools just to make the stack look larger.

## 10. Intelligence and safety principles

Start with reliable evidence collection and rule-based analysis. Add
AI-assisted explanations after the underlying data flow works.

The system should: - Distinguish observed facts from hypotheses. -
Identify metrics/logs/events supporting an explanation. - Avoid
fabricating costs, metrics or root causes. - Recommend actions with
clear rationale. - Never give an LLM unrestricted AWS or Kubernetes
credentials. - Keep recommendations separate from execution. - Require
policy checks and approval for impactful actions. - Verify outcomes
after approved actions.

For the academic MVP, incident analysis and cost analysis can be basic
and bounded. Do not claim advanced autonomous AIOps unless implemented.

## 11. Obsidian Second Brain

Aryan coordinates the shared vault:

`CloudGenome-Second-Brain`

Suggested structure:

``` text
CloudGenome-Second-Brain/
├── 00-Dashboard/
├── 01-Project-Overview/
├── 02-System-Architecture/
├── 03-Team-Responsibilities/
├── 04-Technologies/
├── 05-Implementation/
├── 06-Commands-and-Runbooks/
├── 07-Troubleshooting/
├── 08-Architecture-Decisions/
├── 09-Integration/
├── 10-Evidence-and-Screenshots/
├── 11-Viva-Preparation/
└── 12-Meeting-Notes/
```

All three members contribute: - What they implemented. - Why they
selected a technology. - Commands and configurations. - Test results. -
Errors and solutions. - Architecture decisions. - Screenshots/evidence
references. - Viva questions and answers.

The vault must not contain secrets or credentials. Use a shared
Git-backed vault or another agreed synchronization method, and avoid
maintaining conflicting copies.

## 12. Definition of done for each module

A module is not complete merely because files have been created.

It is complete when: 1. A teammate can follow its README and run it
independently. 2. Its main functionality works. 3. Its Git history shows
the owner's contribution. 4. Its CI/CD workflow has been executed and
verified. 5. Its Docker image builds and runs. 6. Its Kubernetes
deployment works where required. 7. Terraform and Ansible steps have
been tested. 8. Monitoring evidence is available. 9. Security scans have
been run and findings documented. 10. The owner can explain and
demonstrate it without relying on another member. 11. Teardown and
cleanup instructions have been verified.

## 13. Working method for AI assistants

When any team member provides this document to Claude or another
assistant:

-   Treat this file as the shared project context.
-   First identify which member and module the request concerns.
-   Respect module ownership boundaries.
-   Do not edit another member's module.
-   Do not assume proposed features are already implemented.
-   Do not silently change agreed architecture decisions.
-   Clearly label suggestions as proposals.
-   Prefer incremental implementation over generating a huge codebase at
    once.
-   Give commands, expected outputs and verification steps.
-   Explain technical concepts clearly enough for implementation and
    viva.
-   Ask before introducing a major dependency or changing a shared
    interface.
-   Include security, cost and teardown considerations for AWS
    resources.

## 14. Decisions still to be finalized

The following items require team confirmation before implementation:

-   Exact AWS region and account/resource strategy.
-   EC2 instance types and budget limits.
-   Exact K3s topology and node roles.
-   Whether the academic deployment uses one coordinated AWS environment
    or separate temporary environments.
-   Exact Blue-Green traffic-switching mechanism.
-   Exact service ports and API contracts.
-   Exact Prometheus/Grafana placement.
-   Exact CI/CD deployment permissions and IAM roles.
-   Whether AI integration uses a hosted API or local model.
-   Obsidian synchronization method.
-   Integration test and final demo sequence.

Until confirmed, these are open decisions---not established
implementation facts.

------------------------------------------------------------------------

## One-line project summary

**CloudGenome is an AWS-first, modular CloudOps intelligence platform
that combines infrastructure automation, secure CI/CD, Kubernetes
operations, monitoring and evidence-based AI recommendations to help
engineering teams understand and manage cloud environments.**

**Team principle:** Three independent mini-projects, three individually
demonstrable contributions, one integrated CloudGenome platform.
