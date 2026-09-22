# Project Agent Rules

`AGENTS.md` is the agent-facing rule entry point; `AGENTS-CN.md` is its Chinese companion. Keep them aligned and report contradictions.

Workflow: product requirements → Codex design and planning → OpenCode + MiniMax M3 implementation → OpenCode + Kimi review → Codex final audit. These are role assignments, not automatic model routing.

## Product Requirements

`docs/requirements/product.md` defines what the product should do: its purpose, users, goals, user-facing capabilities, constraints and scope.

- Users may explain ideas naturally, in one message or several. Codex maintains the documents for them. Do not require users to write detailed specifications, numbered requirements, acceptance IDs or version records.
- Keep requirements in one product document by default. Do not automatically create per-feature documents, requirement indexes, baseline manifests or traceability matrices.
- Technical decomposition, workflows, data models, APIs, modules, implementation steps and testable acceptance criteria belong in `docs/architecture.md` and `docs/implementation-plan.md`. Codex owns this work.
- Preserve the user's business intent. List unclear business rules under open questions instead of inventing answers. Ask only questions that materially affect the current work; existing explicit instructions do not need repeated confirmation.
- `product.md` describes current intended behavior, not implementation progress. Clearly separate future ideas and unresolved questions from the current scope.
- For important additions, changes or removals, Codex updates product.md and adds a short dated entry to `docs/requirements/change-log.md`: what changed and why, with impact when useful. Git stores detailed history; no per-requirement versioning is required.
- On a requirement change, read both files, assess architectural impact, update architecture and the plan, and record significant technical decisions. Do not silently change requirements to simplify implementation.

## Roles

### Design / Planning — Codex

Read the product requirements, recent changes, existing code and project documents. Select a manageable scope for this round. Produce architecture, implementation steps, affected files, acceptance criteria and relevant checks; state what is out of scope. Architecture and planning may be done together. Mark the plan Ready when the current scope is clear and no blocking questions remain. Stop before business coding.

### Implementation — OpenCode + MiniMax M3

Read product.md, change-log.md, architecture.md, implementation-plan.md and decisions.md under docs (requirements files are under docs/requirements).

- Start only with a Ready plan and an implementation instruction.
- Follow the plan; do not change product intent, architecture, acceptance criteria or scope, and avoid unrelated refactoring.
- Run relevant checks after meaningful stages. Record actual commands, results, changed files and unresolved issues in the plan.
- If requirements change or design conflicts appear, stop the affected part and report it to Codex. Independent work may continue. Do not silently redesign or mix old and new scope.
- Hand off to independent review when implementation is ready; never claim checks passed if they did not run.

### Review — OpenCode + Kimi

Review without modifying code; write only `docs/review-report.md`. Check the product's relevant feature descriptions directly as well as the plan and architecture. Focus on correctness, missing requirements, regression risk, edge cases, error handling, security, tests and unnecessary complexity.

Classify findings as Critical / Major / Minor / Suggestion, with file locations, evidence, impact and advice. Refer to feature names or document headings; formal requirement IDs are unnecessary. Record the reviewed revision and limitations. Recheck fixes before closing findings. Ordinary fixes return to implementation; design conflicts return to Codex.

### Final Audit — Codex

Write `docs/audit-report.md`; give conclusions before any code changes. Check product intent, architecture boundaries, technical debt, complexity, document consistency, test evidence and review resolution. Mark the task Done only when this round is verified and no blockers remain. A completed task does not mean the entire product is complete or authorize deployment.

## Source of Truth

1. `docs/requirements/product.md` (current scope, excluding future ideas and unresolved questions)
2. `docs/architecture.md`
3. Current accepted decisions in `docs/decisions.md`
4. `docs/implementation-plan.md`
5. Existing implementation

Report contradictions. This order does not override explicit user instructions or higher-level runtime rules. The change log explains history; product.md describes the current intended behavior.

## Git and Handoff

Before edits, inspect git status and relevant existing files; preserve unrelated user changes. Before completion, run applicable checks and report changed files, actual results and unresolved issues.

- Record a pre-implementation Git commit in the plan to define the code review range. This is not a separate requirements approval process. If no commit exists, explicitly review all initial files rather than treating an empty diff as success.
- Review committed changes since that point, staged/unstaged changes and untracked files. Read new files explicitly.
- Preserve prior plans and reports in Git history before replacing them. If they are uncommitted, keep a dated copy instead; do not require an archive index or task numbering system. Reset review/audit conclusions for a new task.
- Do not commit, push, deploy or run destructive Git operations without authorization. Do not store secrets or machine-specific private paths in the template.
- Do not automatically delegate or advance to another stage without an explicit instruction.
