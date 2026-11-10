# CLAUDE.md — CloudGenome, Module A (Aryan)

You are working for **Aryan** on **Module A: Cloud Infrastructure, Kubernetes & Second Brain** of CloudGenome.

## Read first, every session
1. `PROGRESS.md` — the current phase and what is verified. This is the source of truth for status.
2. `docs-context/CloudGenome - Common_Project_Context.md` — shared team context.
3. `docs-context/CloudGenome_Aryan_Master_Prompt.md` — Aryan's task.
4. `docs-context/phases/README.md` and the file for the current phase.

Where documents disagree, the later explicit decision from Aryan wins. Known one: **Obsidian is used as the Second Brain product feature** (Markdown vault with `[[wiki-links]]`, tags and frontmatter, served by a CloudGenome API). This overrides the master prompt's "must not depend on Obsidian". Say so if it causes problems.

## Honesty rules (non-negotiable)
- **Never claim something works unless you ran it and saw the output.** Quote the command and the real result.
- Label everything: `PROPOSED`, `IMPLEMENTED`, `TESTED`, or `VERIFIED-ON-AWS`.
- Do not invent metrics, costs, scan findings, versions, flags or API behaviour. If unsure, check the docs or the tool (`--help`, `terraform validate`) or say "I don't know".
- If a test fails, report the failure and the output. Do not hide, skip or weaken tests to get a pass.
- If something was skipped or only partly done, say so plainly.

## Phase workflow
- Work **one phase at a time** in order (0 → 6). Do not start a phase early.
- **Free first.** Test everything locally (no AWS spend) before it touches AWS. Phases 0–3 must be free.
- Before closing a phase: run every item in that phase's **Acceptance tests**, and be completely sure each passes. Re-run from a clean state if there is any doubt.
- At the end of a phase: update `PROGRESS.md`, make the commits, then **stop and wait for Aryan to say "next"**. Never roll into the next phase on your own.
- Prefer small steps. For each step: what and why, exact commands, expected output, how to verify, likely errors, and what to document.

## Progress file (do this without fail)
`PROGRESS.md` must be updated **at the end of every task, every phase and every session** — never batch it up for later. Record: what was done, what was verified (with the command used), what failed, decisions made, the next step, and AWS resources currently live. If you changed something and did not update `PROGRESS.md`, the work is not finished.

## Confusion and fresh starts
- If requirements conflict, the state is unclear, or you are going in circles (e.g. the same error after 2–3 attempts), **stop and tell Aryan plainly**. Do not guess or paper over it.
- If the project has gotten tangled, say "recommend a fresh start" and explain why, what to keep, and what to redo. Do not do a destructive reset without approval.
- Ask before making a major architecture change, adding a major dependency, or changing a shared interface.

## Ownership and Git
- Work only in `module-a-infrastructure/` on branch `aryan-infrastructure`. Do **not** edit `module-b-devsecops/`, `module-c-intelligence/`, or root shared files (root README, integration contracts) without Aryan's agreement.
- Never commit to `main`. Use PRs. Commits should be small and meaningful.
- Commit only when asked or at phase end as agreed. Check `git config user.name` / `user.email` are Aryan's before committing.
- Never push, open PRs, or force-anything without explicit approval.
- Every commit message ends with the attribution line the harness specifies.

## AWS, cost and security
- **Never run `terraform apply`, `destroy`, or any command that creates or deletes AWS resources without showing the plan and getting explicit approval.** Aryan configures credentials; never ask for them in chat.
- Never hardcode or commit secrets, keys, tokens, `.env` files, `*.tfstate`, `*.tfvars` with secrets or `.pem` files. Keep `.gitignore` correct.
- Cheapest suitable resources, a budget alert first, tags on everything, one region (decided in Phase 0).
- Every AWS phase ends with teardown **and a check that nothing is left running**. Record live resources in `PROGRESS.md`.
- Least-privilege IAM. No LLM or service gets unrestricted AWS/Kubernetes credentials.
- Keep module A independently runnable: it must not need Module B or C.

## Manual-only tasks (Aryan does these; do not attempt)
AWS account, billing, IAM and `aws configure`; EC2 key pair; installing tools; GitHub login and repo access; GitHub Actions secrets; reviewing/merging PRs; screenshots from consoles; opening Obsidian; the live demo and viva.

## Style
- Windows host (PowerShell primary, Bash available). Use forward-slash paths in code and containers.
- Explain concepts well enough for Aryan to answer them in a viva.
- Be concise. Link files as `[name](path)`.
