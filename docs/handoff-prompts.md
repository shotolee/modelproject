# 四阶段交接提示词

在项目根目录启动相应工具。每段独立使用，不一次性要求模型跑完全部阶段。开始前补充真实需求，OpenCode 阶段手动确认实际模型。

## 1. Codex：设计与规划

```text
先不要编写业务代码。

读取 AGENTS.md，检查项目目录、git status 和已有文档，分析我提供的需求。
需求：<填写本次真实需求、约束和不做的范围>

完成项目设计并更新：
- docs/architecture.md：目标、边界、模块、数据结构、接口、依赖、错误处理
- docs/implementation-plan.md：步骤、具体文件、验收条件、测试要求、禁止修改范围
- docs/decisions.md：重要技术决策、备选方案和选择原因

不要把占位内容当成已有需求。不确定且影响实施的问题要明确列出。
只有设计和计划完整且无阻塞时将计划设为 Ready，否则保持 Draft 或 Blocked。
说明进入实施前需要记录的 Git 基线。完成后停止，不进入编码阶段。
```

## 2. OpenCode + MiniMax M3：实施

```text
你当前只负责 implementation。
读取 AGENTS.md、docs/architecture.md、docs/implementation-plan.md、docs/decisions.md。
检查 git status、相关代码、计划状态和评审基线；基线缺失或计划未 Ready 时先报告缺项。

严格按计划实施，将状态更新为 In Progress：
- 不重新设计架构，不做计划外重构，不改变验收标准
- 每完成一个有意义的阶段，执行相应测试并在计划中记录命令和结果
- 发现架构或计划问题，停止受影响部分，记录证据并交回 Codex 判断
- 不覆盖无关改动，不擅自提交或推送

完成后总结修改文件、验收和测试结果、未解决问题，并更新计划中的实施记录与交接总结。
满足实施交接条件时设为 In Review，停止并交给独立 reviewer。
```

## 3. OpenCode + Kimi：独立 Review

```text
你现在是独立 reviewer，不要修改代码，仅更新 docs/review-report.md。
读取 AGENTS.md、docs/architecture.md、docs/implementation-plan.md、docs/decisions.md。

根据计划中的基线检查全部本轮变化：相对基线的 diff、暂存和未暂存 diff，
以及未跟踪的新文件。读取相关完整代码和测试上下文，不只看 diff 片段。
检查已有测试证据，必要时运行相关检查；区分实际执行结果与实施者报告。

重点检查计划满足程度、架构偏离、逻辑 bug、边界条件、回归风险、错误处理、
安全、测试充分性和不必要复杂度。

按 Critical / Major / Minor / Suggestion 输出问题，标注稳定编号、文件位置、
证据、影响与建议。记录基线、HEAD、工作区范围和未验证项。
无发现时明确说明实际审查范围和限制。不要重写实现，不要自动修复。
完成后停止。若这是修复后的复核，保留原问题编号并补充复核证据。
```

## 评审后分流

普通实现问题交给 MiniMax 修复，引用 review-report.md 中的编号，保持原计划范围，重新测试并交 Kimi 复核。架构或计划问题交回 Codex，先更新设计再实施。非阻塞建议可留到后续任务，记录处置理由。

## 4. Codex：最终 architecture audit

```text
进行最终架构审计，先给结论，不要直接重写代码。
读取 AGENTS.md、docs/architecture.md、docs/implementation-plan.md、
docs/decisions.md、docs/review-report.md。

核对评审基线、当前 HEAD、工作区及新增文件，检查本轮全部变化和测试证据。
确认评审之后的代码修改是否已复核；未验证事项不能直接视为通过。

重点判断实现是否符合架构、模块边界是否正确、是否新增技术债或过度设计，
以及 architecture.md / decisions.md 是否需要更新。

将结论和依据写入 docs/audit-report.md，更新计划阶段：
- 证据充分、评审和验收完成且无阻塞项：Done
- 实现需修复：回实施阶段并指明问题
- 架构需调整：回设计阶段，先说明需要变更的文档和原因
- 证据不足：明确未验证项，不标记 Done

完成审计后停止，不自动重写、提交、推送或部署。
```
