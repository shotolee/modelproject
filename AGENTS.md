# Project Agent Rules

`AGENTS.md` is the agent-facing rule entry point; `AGENTS-CN.md` is its Chinese companion. Keep them aligned and report contradictions.

Workflow: product requirements → Codex design and planning → OpenCode + MiniMax M3 implementation → OpenCode + Kimi review → Codex final audit. These are role assignments, not automatic model routing.

## Product Requirements

`docs/requirements/product.md` defines what the product should do: its purpose, users, goals, user-facing capabilities, constraints and scope.

- Users may explain ideas naturally, in one message or several. Codex maintains the documents for them. Do not require users to write detailed specifications, numbered requirements, acceptance IDs or version records.
- Users may describe features at a capability level. Do not require them to decompose each feature into detailed sub-requirements. Codex is responsible for functional decomposition during design and planning; clarify consequential business choices instead of inventing them.
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

Git checkpoints are required during development. A request to perform project work authorizes ordinary local task branches and scoped local commits under these rules, unless the user explicitly requests no commits. It does not authorize remote pushes, merges, releases or deployment.

### Start of a task

- Confirm the project root with `git rev-parse --show-toplevel`; do not mistake a parent repository for the new project's repository. Initialize a separate repository if necessary and make an initial template commit before design work. Check `.gitignore` and exclude secrets and generated files first.
- Inspect branch, HEAD, staged/unstaged changes and untracked files. Do not automatically stage, commit, discard or stash unrelated user work. If it overlaps the task, resolve ownership with the user before touching that part; independent work may continue.
- Use a task branch such as `work/<short-topic>` created from the intended starting commit. Reuse the current branch if it already belongs to this task. Do not automatically merge into the default branch.
- Record the starting branch/commit in the plan. Before replacing previous plans or reports, ensure their contents are committed or preserved in a dated copy. Reset review/audit conclusions for the new task.

### Stage checkpoints

1. **Codex design:** check document consistency, commit the task's requirements, architecture, decisions and Ready plan together (`docs: ...`). This design commit is the fixed code-review baseline; the implementation agent records its SHA before coding. Record an actual existing SHA, never a predicted hash for the commit containing that record.
2. **MiniMax implementation:** divide work into the meaningful steps in the plan. After each step, inspect the diff, run relevant checks, update the implementation record and make a scoped commit (`feat: ...`, `fix: ...`, `test: ...`). Do not wait until the entire feature is finished to commit.
3. **Blocked or interrupted work:** record failed checks, incomplete scope and the next action. If changes must be saved before resolution, use a clearly marked `wip: ...` checkpoint; it is not a passed stage and cannot be handed off as completed. Unavailable checks must be reported, not treated as passing.
4. **Kimi review:** review baseline → explicit implementation commit and check the working tree as well. Record both SHAs, actual checks and findings; commit only the review report (`docs: review ...`). Report commits do not change the reviewed implementation revision. Code, tests, configuration, requirements, architecture or plan-scope changes after review require relevant revalidation; report/evidence-only updates do not by themselves invalidate review.
5. **MiniMax fixes:** commit each coherent fix after checks and have Kimi review the new implementation revision. Keep the original baseline fixed throughout this task.
6. **Codex audit:** record the implementation revision and review evidence, then commit the audit and final task records (`docs: audit ...`). Mark Done only when checks and review pass, blockers are resolved, and all task changes have been committed.

At each commit, inspect `git diff`, selectively stage task paths or hunks, check `git diff --cached` and `git diff --cached --check`, commit, then verify HEAD and status. Avoid blanket `git add .` / `git add -A`. Leave unrelated staged changes intact; use an isolated index if needed or resolve the staging conflict before committing. Do not accidentally include them in the task commit.

At handoff, provide the branch, baseline, implementation/checkpoint SHAs, verification results, remaining work and any dirty paths. A handoff must have no uncommitted task changes; unrelated changes are listed separately. If commits are explicitly disallowed, or Git identity/hooks prevent them, explain the checkpoint gap and do not claim the stage is complete or the task Done. Do not bypass hooks or change global identity settings without authorization.

Use ordinary follow-up commits to correct errors and `git revert` for an authorized rollback; do not automatically amend, reset hard, clean, force-push or rewrite history. Git provides local history, not an off-device backup. Push to the named remote only when authorized, and verify the result. Do not store secrets or machine-specific private paths in the template.
- Do not automatically delegate or advance to another stage without an explicit instruction.
