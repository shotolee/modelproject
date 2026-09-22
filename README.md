# 多模型协作项目模板

供 Mac mini 和 MacBook Air 复制使用的通用项目骨架。先用一个小功能手动跑通四阶段，再考虑自动化。模板不预设业务、技术栈或运行环境。

## 目录

```text
AGENTS.md                       所有 Agent 的共同规则
docs/
  architecture.md               当前有效架构
  implementation-plan.md        当前任务、步骤、验收与测试证据
  decisions.md                  技术决策及原因
  review-report.md               Kimi 独立评审及复核
  audit-report.md                Codex 架构终审
  handoff-prompts.md             四阶段可复制交接提示词
```

## 从模板创建项目

把模板文件复制到一个新目录，**不要复制 `.git`**，然后在新目录初始化独立仓库。例如在模板目录执行（替换目标路径；目标目录应尚不存在）：

```bash
mkdir -p "$HOME/projects"
mkdir "$HOME/projects/my-new-project"
rsync -av --exclude='.git' --exclude='.DS_Store' ./ "$HOME/projects/my-new-project/"
cd "$HOME/projects/my-new-project"
git init -b main
```

随后给 Codex 提供真实需求：项目目标、使用者、必须实现的行为、约束与不做的范围。用 `docs/handoff-prompts.md` 中的第一阶段提示词开始设计。初始文档全部是占位模板，不能直接据此实施。

设计完成后检查文档。设计无阻塞问题时由 Codex 将计划设为 `Ready`。进入实施前，保存设计基线（以下提交由你手动执行）：

```bash
git add AGENTS.md README.md .gitignore docs/
git commit -m "docs: establish project design and workflow"
git rev-parse HEAD
```

将输出的完整 commit SHA 填入实施计划的“评审基线”。不要把复制来源仓库的历史或某次旧任务的报告带入新项目。

## 四阶段交接

| 阶段 | 负责人 | 主要产出 | 结束条件 |
| --- | --- | --- | --- |
| 设计 | Codex | 架构、计划、决策 | 计划 Ready，无设计阻塞；停止编码前 |
| 实施 | OpenCode + MiniMax M3 | 代码、测试、实施记录 | 计划进入 In Review，范围和证据齐全 |
| 独立评审 | OpenCode + Kimi | review-report.md | 问题有明确结论；必要时修复并复核 |
| 架构终审 | Codex | audit-report.md | 给出通过、需修复或回设计结论 |

切换到 OpenCode 后，手动确认实际选中的模型。这里不更改 OpenCode、oh-my-openagent 或 Codex 的全局配置，也不实现 `proj-plan` 等命令。5700G、本地模型、ASR 和 RAG 留待后续阶段。

计划状态流转：`Draft → Ready → In Progress → In Review → In Audit → Done`。遇到阻塞可设为 `Blocked`，并写清阻塞部分和恢复条件。评审发现问题则回 `In Progress`，架构问题回 `Draft`；修复后重新评审。Suggestion 不自动扩大当前任务范围。

只有对应版本通过评审、验收证据齐全、终审通过且没有阻塞项时，才标记 `Done`。阶段由人工交接，不自动串行调用模型。

## 如何确定评审范围

评审基线必须是本轮实施前的 commit。以下命令中的 `<baseline-sha>` 要替换为计划中记录的真实 SHA：

```bash
git status --short
git diff --stat <baseline-sha>
git diff <baseline-sha>
git diff --cached
git diff
git ls-files --others --exclude-standard
```

相对基线的 diff 用于覆盖已经提交的实现及当前受跟踪文件变化；最后一条列出新增但未跟踪的文件，reviewer 必须另行读取。报告记录当时的 HEAD、基线及工作区状态；报告生成后代码发生变化，相关结论需复核。

模板自身没有业务代码和测试命令。项目设计阶段必须填写真实的验证方式，不能预设 `npm test` 等命令能运行。
