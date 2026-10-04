# Project Agent Rules

`AGENTS.md` is the agent-facing rule entry point; `AGENTS-CN.md` is its Chinese companion. Keep them aligned and report contradictions.

Workflow: product requirements → one primary agent owns planning and implementation → an independent session reviews a fixed revision → the primary agent accepts the result. Codex is the default primary agent; MiniMax and Kimi are optional contributors or reviewers. Roles do not require different model brands or automatic model routing.

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

## Roles and Continuity

### Primary Agent — Codex by default

Maintain current product intent, design, task status, issue dispositions and final acceptance throughout the task. Plan a manageable increment and specify acceptance and verification. Mark Ready only when scope and verification are clear and no blocking questions remain; commit the Ready design before implementation. When only design is requested, stop there. When implementation is authorized, the primary agent may code without a mandatory handoff to another model.

Prefer the same implementation session for coherent work and ordinary fixes. If a contributor is used, the primary agent supplies the bounded task and integrates its delivery. Delegate or automatically advance only within explicit user authorization; this template does not launch agents itself. Independent review must come from a session that did not implement the task. Primary acceptance is not a substitute for that review and does not authorize deployment.

### Implementer — Primary Agent or a Selected Contributor

Read the current task summary, applicable product/design sections and relevant code; inspect the actual Git state. Before editing, briefly state the understood goal, scope, invariants, reviewed/checkpoint revision and next action. Resolve contradictions before affected work; no routine user confirmation is needed when the task is already clear.

Start only with a Ready plan and implementation authorization. Follow agreed product intent, architecture and acceptance; preserve closed fixes and public contracts. Verify each meaningful step, commit it, and update actual results. Do not silently redesign, expand scope or equate test counts with contract coverage. Preserve remaining work, failed approaches and their reasons when the session changes.

### Independent Reviewer — A Separate Session, Any Suitable Model

Review a fixed implementation commit, not a changing working tree. Read the relevant product intent and acceptance directly, the complete task diff and affected call sites. Write only `docs/review-report.md`; do not fix code or edit the plan. Classify findings Critical / Major / Minor / Suggestion and distinguish an implementation defect, a required evidence gap, and an optional improvement.

Every blocking finding needs a requirement/acceptance reference, file location and concrete defect or missing mandatory evidence. A new preference does not become a mandatory requirement during review. New verified safety/correctness defects may block even if the plan omitted them; the primary agent records their impact without hiding or weakening them. Recheck changes and affected regressions; closed findings stay closed unless related changes or new evidence justify reopening them.

### Acceptance — Primary Agent

Verify the committed review report, implementation version, blocking issue closure, relevant tests, architecture and document consistency. Record the final decision in `docs/audit-report.md`. Do not blindly rerun the review or all tests; investigate uncertain or conflicting evidence. Mark Done only when the task is independently reviewed, verified, committed and has no unresolved blockers. A partial task is not the whole product.

## Collaboration Controls

- **One current summary:** keep a concise current-state block at the top of `docs/implementation-plan.md`: goal, scope, files allowed, invariants, branch/baseline/revision, completed work, open blockers, failed approaches and next action. The primary agent owns this block; a contributor may append evidence only in its assigned section. Current architecture and plans describe valid decisions, not cumulative transcripts. Preserve history in Git before condensing superseded content; read relevant historical entries only when needed.
- **One writer at a time:** name the active role/session and stage in the plan. In a shared checkout, only that designated session changes task files. The primary agent stops edits while a contributor or reviewer is active. Never start a duplicate job or interrupt ongoing work because a heartbeat or retry fired. Role transfer requires verified completion/stoppage and reconciled files. Independently authorized parallel work needs separate worktrees and an explicit integration owner.
- **Immutable delivery:** before review, commit all task changes and pass the fixed baseline and implementation SHA. During review, freeze task code, tests, configuration, requirements, design and plan scope; only the reviewer writes its report. If those inputs change, pause and restart/reconcile review against the new revision. Unrelated dirty user paths remain untouched and are disclosed; use an isolated checkout if they affect review.
- **Verify the report artifact:** the reviewer writes, reads back, commits and verifies the report against its report commit. The primary agent reads that exact committed report and checks its conclusion and reviewed SHA, rather than relying on a chat summary or mutable file. Report commits do not change the reviewed implementation SHA. Report mismatch means Pending; determine the source and correct delivery before acceptance. Never guess which agent overwrote a file. No self-referential commit hashes are required.
- **Two unsuccessful repair rounds:** count attempts per stable issue ID (for example R-01), not per model/session. After two delivered repairs fail independent revalidation of the same blocker, stop automatic retries. The primary agent reproduces the problem, checks the acceptance interpretation and makes a bounded decision: a smaller executable repair, a suitable implementer, or an explicit design correction. Record the decision before resuming; changing sessions does not reset the count or weaken acceptance. Ask the user only for a material business choice or external access. New unrelated issues are counted separately.
- **Risk-based verification:** repair review checks the specific issue, relevant regressions and contract preservation. Reopen full review when scope, shared contracts or new evidence warrants it. Repeat full/race/build/integration checks when affected dependencies or unresolved failures justify them, not for unchanged comments alone. Reused evidence states its command, result, revision and applicability; never call it freshly executed. Unavailable checks and residual risks remain explicit.
- **No model lock-in:** choose by demonstrated task suitability and known availability, preserving implementation/review separation. If quota fails, a contributor makes no observable progress within a task-specific checkpoint deadline, or delivery repeatedly misses scope, stop duplicate retries and have the primary agent choose a continuation. Set the progress checkpoint in the plan when outsourcing; status text alone is not delivery. Do not poll quota failures repeatedly or kill an active task blindly. Reconcile real process/session status, files and last commit before transfer. Record availability as unknown when unavailable; the template does not impose quota probes on every local edit.
- **Bounded findings:** the reviewer recommends severity; the primary agent records blocking decisions and deferred items. Confirmed critical defects and unresolved major correctness/security/contract or mandatory-evidence gaps block acceptance. Minor/Suggestion normally remain nonblocking; a mandatory gap must have a stated blocking reason. Do not make every suggestion a new FIX round. Fixes cannot silently lower tests or redefine product intent.

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

1. **Primary-agent design:** check document consistency, commit the task's requirements, architecture, decisions and Ready plan together (`docs: ...`). This design commit is the fixed code-review baseline; the implementation agent records its SHA before coding. Record an actual existing SHA, never a predicted hash for the commit containing that record.
2. **Implementation:** divide work into the meaningful steps in the plan. After each step, inspect the diff, run relevant checks, update the implementation record and make a scoped commit (`feat: ...`, `fix: ...`, `test: ...`). Do not wait until the entire feature is finished to commit.
3. **Blocked or interrupted work:** record failed checks, incomplete scope and the next action. If changes must be saved before resolution, use a clearly marked `wip: ...` checkpoint; it is not a passed stage and cannot be handed off as completed. Unavailable checks must be reported, not treated as passing.
4. **Independent review:** review baseline → explicit committed implementation revision and confirm the checkout matches it. Record both SHAs, checks and findings; write, read back and commit only the review report (`docs: review ...`). The primary agent reads the exact report commit before deciding. Report commits do not change the reviewed implementation revision. Code, tests, configuration, requirements, architecture or plan-scope changes after review require relevant revalidation; report/evidence-only updates do not by themselves invalidate review.
5. **Fixes:** commit each coherent fix after checks and have the independent reviewer revalidate the new implementation revision. Keep the original baseline fixed throughout this task.
6. **Primary-agent acceptance:** record the implementation revision and review evidence, then commit the audit and final task records (`docs: audit ...`). Mark Done only when checks and review pass, blockers are resolved, and all task changes have been committed.

At each commit, inspect `git diff`, selectively stage task paths or hunks, check `git diff --cached` and `git diff --cached --check`, commit, then verify HEAD and status. Avoid blanket `git add .` / `git add -A`. Leave unrelated staged changes intact; use an isolated index if needed or resolve the staging conflict before committing. Do not accidentally include them in the task commit.

### Recovery checkpoints

- Do not rely on chat history, autosave or the working tree as the only record. In addition to completed-step commits, save scoped progress before a planned pause, session/model transfer, context handoff, long-running operation or broad/risky change. Commit incomplete work as `wip: ...`, with completed work, failed/unrun checks and the next action in the plan. Only the designated writer saves task changes; reviewers retain their report-only scope.
- For extended editing, set a checkpoint cadence in the plan (default: about 30 minutes of active editing). At the next safe boundary, save meaningful unsaved progress even if the step is incomplete. Do not interrupt running tools or create empty commits just to meet the clock. Small steps that already commit within this window need no extra checkpoint.
- After an interruption, inspect branch, HEAD, recent commits, tracked diff and untracked files; compare the actual state with the last checkpoint and plan. Preserve recovered changes first, reconcile unfinished processes and write ownership, then resume the recorded next action and rerun checks affected by incomplete work. Never automatically reset, clean, switch away from dirty work or treat a WIP checkpoint as verified.
- A checkpoint covers only inspected and committed files; secrets and ignored/generated data are excluded. When remote backup is authorized, push checkpoint commits to the named task branch and verify the remote SHA. Without that authorization, report the latest local SHA and that off-device backup is pending. An abrupt crash can still lose changes since the last checkpoint; do not promise zero loss.

At handoff, provide the branch, baseline, implementation/checkpoint SHAs, verification results, remaining work and any dirty paths. A handoff must have no uncommitted task changes; unrelated changes are listed separately. If commits are explicitly disallowed, or Git identity/hooks prevent them, explain the checkpoint gap and do not claim the stage is complete or the task Done. Do not bypass hooks or change global identity settings without authorization.

Use ordinary follow-up commits to correct errors and `git revert` for an authorized rollback; do not automatically amend, reset hard, clean, force-push or rewrite history. Git provides local history, not an off-device backup. Push to the named remote only when authorized, and verify the result. Do not store secrets or machine-specific private paths in the template.
- Do not automatically delegate or advance to another stage without an explicit instruction.
