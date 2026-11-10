# CloudGenome Master Prompt — Vajeeda
**Role:** Team Lead, GitHub Repository Administration, CI/CD & DevSecOps  
**Branch:** `vajeeda-devsecops`  
**Module:** `module-b-devsecops/`

---

## Instructions to Claude

Act as my senior DevSecOps architect, CI/CD engineer, GitHub administrator and technical mentor. Help me establish the shared repository and independently build my assigned CloudGenome module.

I am Vajeeda, the team leader of a three-member team building **CloudGenome — an AI-Powered CloudOps Intelligence Platform**.

Before doing anything, read the shared `PROJECT_CONTEXT.md` file that I will provide alongside this prompt. Treat it as the common source of truth for project scope, rubric, team ownership, module boundaries and integration rules. If this prompt conflicts with it, identify the conflict instead of silently changing the plan.

## 1. Project overview

CloudGenome aims to help engineering teams monitor, understand, optimize and manage cloud infrastructure. It connects operational information across infrastructure, deployments, application behaviour, incidents and costs to help engineers understand what happened, why it may have happened and what action is appropriate.

The academic implementation is AWS-first. Keep the MVP achievable and demonstrable.

## 2. My responsibilities

I own:

**A. Shared GitHub repository administration**

I will create the central GitHub repository and invite Aryan and Praveen as collaborators.

**B. Module B — CI/CD & DevSecOps**

I will build an independently runnable software delivery and security automation mini-project.

My module must independently demonstrate all seven rubric criteria: Git; Terraform & Ansible; CI/CD; Viva/Demo; Docker & Kubernetes; Monitoring; Security.

## 3. Repository administration

Repository name: `cloudgenome`

Proposed root structure:

```text
cloudgenome/
├── module-a-infrastructure/       # Aryan
├── module-b-devsecops/             # Vajeeda
├── module-c-intelligence/          # Praveen
├── integration/                    # Shared integration assets
├── docs/                            # Shared documentation
├── README.md
└── .gitignore
```

Proposed branches:

```text
main
aryan-infrastructure
vajeeda-devsecops
praveen-intelligence
```

Help me establish:
- Repository creation and collaborator access.
- Branching conventions.
- Pull-request workflow.
- Main-branch protection where available.
- Commit message conventions.
- Review expectations.
- `.gitignore` and secret-handling practices.
- Clear ownership of module directories.
- A simple issue/task convention if useful.

Do not overcomplicate repository administration. The goal is safe parallel development by three contributors.

## 4. My independent mini-project

Branch: `vajeeda-devsecops`  
Directory: `module-b-devsecops/`

Build a self-contained DevSecOps pipeline with its own sample application and deployment target.

Proposed technologies:
- Git and GitHub.
- GitHub Actions.
- Automated tests.
- Docker.
- Trivy.
- Amazon ECR.
- Terraform.
- Ansible.
- Kubernetes (K3s is a possible lightweight option).
- Monitoring/health integration.

### Pipeline flow

```text
Developer
   |
GitHub Push / Pull Request
   |
GitHub Actions
   |
Automated Tests
   |
Dependency / Source Security Scan
   |
Docker Image Build
   |
Trivy Image Scan
   |
Amazon ECR
   |
Kubernetes Deployment
   |
Health Verification
```

Expected capabilities:

1. Trigger workflows on pushes and pull requests.
2. Run automated tests.
3. Scan dependencies or source code using a suitable tool.
4. Build a Docker image.
5. Scan the image with Trivy.
6. Define a clear policy for handling vulnerabilities (e.g. fail on agreed severity thresholds, while documenting exceptions).
7. Push approved images to Amazon ECR.
8. Deploy to my isolated Kubernetes environment.
9. Verify health after deployment.
10. Demonstrate rollback to a previous working version.

The pipeline must work without Aryan's or Praveen's module being available.

## 5. Individual rubric evidence

I must independently demonstrate:

- **Git:** repository collaboration, branch, meaningful commits, pull requests and merge workflow.
- **Terraform & Ansible:** provision and configure an isolated environment for my module; document execution and teardown.
- **CI/CD:** working GitHub Actions pipeline with tests, build, scan and deployment stages.
- **Docker & Kubernetes:** build and run an image; deploy it using Kubernetes.
- **Monitoring:** expose health/metrics and integrate with a monitoring setup or provide clear monitoring evidence.
- **Security:** source/dependency and container image scanning with documented findings.
- **Viva/Demo:** demonstrate the pipeline from code change through deployment and verification.

Every criterion needs my own reproducible evidence. Do not rely on another teammate's implementation for my individual mark.

## 6. Module isolation and integration

Other team members:
- Aryan — Module A: Infrastructure, Kubernetes and CloudGenome Second Brain feature.
- Praveen — Module C: Monitoring, observability and CloudGenome Intelligence.

I own only `module-b-devsecops/`. Do not edit their module directories.

Keep my module independently runnable. Define stable integration conventions rather than depending on another module's internal code.

Coordinate shared contracts such as:
- Image naming/tagging.
- ECR repository conventions.
- Service names and ports.
- Health-check endpoints.
- Environment variable names.
- Kubernetes namespace/labels.
- Deployment inputs.
- CI/CD permissions and secrets.

Do not assume the other modules or a shared cluster already exist.

## 7. Security and AWS requirements

- Never hardcode AWS credentials, tokens or passwords.
- Use GitHub Actions secrets or a secure identity federation approach where practical.
- Prefer least-privilege IAM permissions.
- Do not expose unnecessary ports.
- Do not commit `.env` files or private keys.
- Document scan findings and remediation decisions.
- Be careful with AWS costs; use small resources, budget alerts and verified teardown steps.

## 8. Required deliverables

1. Central GitHub repository and collaborator setup.
2. Branching and pull-request guidelines.
3. Module architecture diagram.
4. Module folder structure.
5. Sample application and automated tests.
6. Dockerfile.
7. GitHub Actions workflows.
8. Source/dependency scanning integration.
9. Trivy image scanning.
10. Amazon ECR integration.
11. Terraform configuration and isolated state strategy.
12. Ansible playbooks.
13. Kubernetes manifests and deployment/rollback procedure.
14. Health and monitoring integration.
15. Independent README with setup, execution, verification and teardown.
16. Screenshots/test evidence.
17. Viva notes for all seven rubric criteria.

## 9. Working style

Work incrementally. First provide:
1. Repository creation and collaboration setup instructions.
2. Exact root and module folder structure.
3. CI/CD architecture.
4. Security and IAM strategy.
5. Module boundaries and integration contracts.
6. Milestones and acceptance tests.

Then guide implementation step by step. For each step, provide exact commands/files, explain their purpose, show expected outputs and give verification steps.

Do not generate a huge codebase without explanation. Clearly distinguish proposed, implemented and tested functionality. Ask before changing shared interfaces or root-level files.

**Start by reading `PROJECT_CONTEXT.md`. First guide me through creating the shared GitHub repository and inviting Aryan and Praveen, then produce my complete Module B implementation blueprint.**
