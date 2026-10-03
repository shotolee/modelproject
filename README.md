# 个人 AI 项目模板

你负责说明“想做什么”，Codex 负责整理需求、细化功能、设计架构和实施计划，再交给 MiniMax 实施、Kimi 评审，最后由 Codex 终审。

## 目录

```text
AGENTS.md                     Agent 共用规则（英文）
AGENTS-CN.md                  中文配套说明
docs/
  requirements/
    product.md               当前产品需求
    change-log.md            重要需求变化及原因
  architecture.md            Codex 编写的架构与功能设计
  implementation-plan.md     本轮实施步骤、验收和测试记录
  decisions.md               重要技术决策
  review-report.md            Kimi 独立评审
  audit-report.md             Codex 终审
  handoff-prompts.md          可复制的操作提示词
```

## 你需要做什么

直接告诉 Codex：这是什么项目、谁使用、想要哪些功能、有什么限制、这次先做什么。可以口述、粘贴已有材料、分批补充，不需要先写完整需求规格。

Codex 会把想法整理进 [product.md](docs/requirements/product.md)，只追问影响当前工作的关键问题。功能怎么拆、页面和流程怎么设计、数据和接口如何组织、怎么验收，都由 Codex 写入架构和实施计划。

需求默认只有两个文件，不强制拆功能文档、编需求编号、维护版本号或追踪表。产品文档回答“做什么”，架构和计划回答“如何实现、怎样验证”。

## 从模板创建项目

可以在 [modelproject 仓库页面](https://github.com/shotolee/modelproject) 点击 **Use this template → Create a new repository**，填写新项目名称并选择可见性。创建后克隆新仓库，再按下方流程开始。

也可以在本地模板目录复制文件，替换目标项目名称；目标目录应尚不存在：

```bash
mkdir -p "$HOME/projects"
mkdir "$HOME/projects/my-new-project"
rsync -av --exclude='.git' --exclude='.DS_Store' ./ "$HOME/projects/my-new-project/"
cd "$HOME/projects/my-new-project"
git init -b main
git add AGENTS.md AGENTS-CN.md README.md .gitignore docs/
git diff --cached --check
git commit -m "chore: initialize project template"
```

然后按 [交接提示词](docs/handoff-prompts.md) 开始。所有占位内容需要由真实项目需求替换，不能直接当成完成的设计。模板不预设技术栈或测试命令。

## 日常流程

| 步骤 | 负责人 | 做什么 |
| --- | --- | --- |
| 说明需求 | 你 → Codex | 自然语言描述，Codex 整理 product.md；大需求可先只整理 |
| 设计与计划 | Codex | 细化本轮功能、架构、实施步骤和验收，完成后停在编码前 |
| 实施 | OpenCode + MiniMax M3 | 按计划编码、测试，记录结果 |
| 独立评审 | OpenCode + Kimi | 核对产品意图、架构和实现，输出问题清单 |
| 终审 | Codex | 检查架构及评审处置，给出结论 |

手动切换工具和模型。发现普通实现错误交 MiniMax 修复后复核；设计冲突回 Codex。无需分别启动需求审批、架构审批和计划审批；已有明确业务要求可直接用于设计，关键歧义才需追问。设计和实施仍分开交接。

## 以后新增或调整功能

只需说：“增加邮件通知，可以选择是否启用。先更新需求和设计，不编码。”

Codex 更新 product.md，在 change-log.md 按日期记下重要变化与原因，分析影响并调整架构、计划及必要决策。你也可以直接编辑产品文档，再让 Codex处理新增内容。不必写新需求文件或自己拆任务。

产品文档保持当前预期；暂不做和待确认事项单独列明。本轮计划用功能名称或产品章节说明范围，不把整个产品都塞进一次实施。中途需求变化先暂停受影响工作，更新设计后再继续。

## 历史与验证

Git 是每个开发阶段的必做步骤，由 Agent 执行。你发起项目任务即授权本任务的普通本地分支与提交，无需每一步手动提醒；明确要求不提交时，Agent 应说明无法形成检查点。远程推送、合并和部署需要对应授权。

| 检查点 | 执行要求 | 产出 |
| --- | --- | --- |
| 初始化 | 确认独立 Git 根目录、忽略规则，提交初始模板 | 可以回到开发前的起点 |
| 任务开始 | 检查分支、HEAD 与已有改动，进入或复用任务分支 | 本任务与其它工作分开 |
| 设计完成 | 检查并提交需求、架构、决策和 Ready 计划 | 固定代码评审起点 |
| 每个实施步骤 | 查看 diff → 运行相关验证 → 更新记录 → 选择性暂存 → 提交 | 可追溯的阶段实现与结果 |
| 独立评审 | 检查起点到明确实施版本，报告引用 SHA；单独提交报告 | 明确哪个版本经过检查 |
| 修复与复核 | 修复验证后提交，reviewer 对新实施版本复核 | 不混用旧版通过结论 |
| 终审与交接 | 检查版本、测试及提交是否齐全，提交审计与最终记录 | 本任务无未提交改动 |

默认任务分支如 `work/功能主题`，阶段提交示例为 `docs: plan ...`、`feat: ...`、`fix: ...`、`docs: review ...`、`docs: audit ...`。先验证再提交，不把整个功能积攒成一次提交。中断或受阻时可用 `wip: ...` 保存，但必须注明失败检查和未完成内容，不能宣称阶段通过。

设计提交是本轮固定评审起点，由实施者在编码前登记其 SHA；不能预测包含记录本身的提交哈希。评审和审计报告引用实施版本，报告提交自己的 SHA 在交接时给出即可。后续代码、测试、配置、需求或设计范围改变需要相关复核；纯报告更新不等于实施版本变化。

详细历史交给 Git；变更日志解释需求“为什么改”，计划记录实施与验证。旧计划和报告在覆盖前先提交或保留日期副本。每次只暂存本任务的文件或片段，核对 staged diff，保留无关用户改动，尤其不能把用户原有暂存变化一起提交。模板不新增需求编号或独立 Git 台账。

评审常用命令（将 `DESIGN_COMMIT_SHA` 和 `IMPLEMENTATION_COMMIT_SHA` 替换为实际 SHA）：

```bash
git status --short
git log --oneline DESIGN_COMMIT_SHA..IMPLEMENTATION_COMMIT_SHA
git diff DESIGN_COMMIT_SHA IMPLEMENTATION_COMMIT_SHA
git diff --cached
git diff
git ls-files --others --exclude-standard
```

同时核对工作区差异，新增文件单独读取。交接提供分支、起点、检查点/实施 SHA、验证结果、遗留问题及未提交文件列表。提交因身份、hooks 等原因失败时如实说明，不绕过保护或宣称 Done。

本地 Git 历史用于跟踪与恢复，异地备份需要推送到远程。获得推送授权后 Agent 再执行并核对远端；不自动合并、强推或部署。回退优先采用经授权的 `git revert`，保留阶段历史。
