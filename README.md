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

详细历史使用正常 Git 提交保存。变更日志只解释重要需求“为什么改”；计划记录实施与测试，评审和终审报告保留结果。替换上一轮文档前，确保旧内容已在 Git 历史中；尚未提交则先保留带日期的副本，无需固定归档体系。

实施前由 Agent 在计划里记录 Git 起点，便于评审已提交和未提交的变化。新仓库尚无提交时，应明确检查全部初始文件。已有起点时，将下列占位符替换为实际 commit：

```bash
git status --short
git diff <实施前commit>
git diff --cached
git diff
git ls-files --others --exclude-standard
```

最后一条列出的新增文件需单独读取。未运行的检查必须如实说明；新任务不能沿用上一轮通过结论。提交、推送和部署按用户指令执行。
