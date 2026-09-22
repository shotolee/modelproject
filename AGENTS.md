# Project Agent Rules

本项目采用四阶段工作流：Codex 设计 → OpenCode + MiniMax M3 实施 → OpenCode + Kimi 独立评审 → Codex 架构终审。
这些规则适用于所有参与项目的 AI Agent。模型名称是职责约定，不会自动选择模型或触发其他 Agent。

## Roles

### Architecture / Planning — Codex

- 负责架构设计、实施计划、重要技术决策及最终架构审计。
- 设计阶段只更新项目文档，不编写业务代码；完成设计后停止。
- 架构变化必须先更新 `docs/architecture.md`、`docs/decisions.md` 及受影响的计划，再交给实施阶段。
- 不得将占位模板、候选方案或尚未解决的阻塞问题当成已批准设计。

### Implementation — OpenCode + MiniMax M3

1. 读取本文件及 `docs/architecture.md`、`docs/implementation-plan.md`、`docs/decisions.md`。
2. 仅在计划状态为 `Ready` 且已收到实施任务时开始编码。
3. 严格遵守计划、架构及明确范围，不做计划外重构。
4. 每完成一个有意义的阶段，运行相关测试，将命令、结果及限制记入计划中的实施记录。
5. 如果发现架构或计划冲突，停止受影响部分，在计划中记录问题、影响及需要 Codex 判断的事项；不自行重新设计。独立且不受影响的步骤可继续。
6. 不得自行修改架构、技术决策、验收标准或扩大计划范围来适配实现。
7. 完成时总结修改文件、测试证据、未解决问题，并交给独立 reviewer。

### Review — OpenCode + Kimi

- 首轮独立审查，不修改代码；仅写入 `docs/review-report.md`。
- 检查正确性、架构偏离、需求遗漏、回归风险、错误处理、安全问题、测试覆盖及不必要复杂度。
- 问题按 `Critical / Major / Minor / Suggestion` 分类，包含文件位置、证据、影响与建议。
- 明确实际评审的基线、当前版本、暂存/未暂存变化及新增文件。不能仅凭空的 `git diff` 判定没有变化。
- 不把“未执行测试”写成“测试通过”；无问题时明确写出审查范围和剩余风险。
- 只有收到明确修复指令后才可修改实现；默认由实施 Agent 修复，并由 reviewer 复核。

### Final Architecture Audit — Codex

- 结合实现、测试及独立评审，判断架构符合性、模块边界、技术债、过度设计及文档一致性。
- 将结论写入 `docs/audit-report.md`，先给审计结论，不直接重写代码。
- 实现错误回实施阶段；架构问题回设计阶段，文档更新后再重新实施及评审。
- 终审不替代独立评审，也不能把未验证的问题直接标为已解决。

## Source of Truth

项目技术事实按以下顺序判断：

1. `docs/architecture.md`
2. `docs/decisions.md` 中仍有效的决策
3. `docs/implementation-plan.md`
4. 现有实现

这是项目文档的一致性规则，不覆盖用户当前的明确指令或运行环境的上级规则。发现冲突须报告并由设计阶段协调；不能仅凭优先级静默忽略另一份文档。

## Git and Verification

修改前检查 `git status`、相关代码及已有变更，不覆盖无关的用户修改。

- 每轮任务在实施前记录基线 commit；新项目应先提交设计文档以获得基线。
- 评审同时覆盖相对基线的已提交变化、暂存/未暂存变化及未跟踪文件；新增文件需实际读取。
- 不擅自提交、推送、合并、部署或执行破坏性 Git 操作。
- 完成前运行适用检查，报告变更文件、测试结果和未解决事项。
- 没有测试基础设施或环境不足时，明确记录未验证项及原因，不能宣称通过。
- 不将密钥、令牌、机器专属路径或私人配置写入模板及版本库。

## Handoff

当前任务、范围、阶段、基线及实施证据记录在 `docs/implementation-plan.md`。
评审和终审分别使用 `docs/review-report.md` 与 `docs/audit-report.md`。
同一时刻保持一个明确的活动任务；进入下一任务前保留上一轮报告（通过 Git 历史或归档），避免混用不同版本的结论。
未经用户明确要求，不自动调度其他 Agent 或推进下一阶段。
