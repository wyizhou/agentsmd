# Agent 协作基础 Harness

这是一个通用的 Agent 协作基础 Harness 工程：通过可复用的协作规则、计划和记忆模板，帮助多个 Agent 按明确分工推进项目，让目标、实施、验证和交付有据可查。实际的子 Agent 创建、调度和并行执行依赖所使用的 Agent 平台。

| 文件 | 用途 |
| --- | --- |
| [AGENTS.md](./AGENTS.md) | 回答偏好、角色分工、协作流程与验收交付约定。 |
| [PLAN.md](./PLAN.md) | 总计划空模板，用于记录总目标、范围、大功能预期与验收标准、依赖、状态和执行计划链接。 |
| [exec-plans/template.md](./exec-plans/template.md) | 单个功能的执行计划空模板，用于记录阶段任务、检查证据、当前检查点和失败记录。 |
| [MEMORY.md](./MEMORY.md) | 项目记忆空模板，用于保存已确认的决定、已验证的事实和可复用经验。 |
| [subagent-templates/planner.md](./subagent-templates/planner.md) | Planner 任务模板，用于首次规划或规划调整。 |
| [subagent-templates/developer.md](./subagent-templates/developer.md) | Developer 任务模板，用于正常开发或问题修复。 |
| [subagent-templates/validator.md](./subagent-templates/validator.md) | Validator 任务模板，用于首次验证、问题复现或修复后复验。 |
| [README.md](./README.md) | 仓库说明、使用方法与 MIT 授权。 |

## 协作方式与优势

- **多角色分工**：主 Agent 统筹，多个子 Agent（subagents）分别承担规划、开发和验证职责；每次派发、修复和复验都使用全新子 Agent，按明确的任务材料工作。
- **有条件的并行协作**：不同功能在依赖满足、接口明确、写入和测试资源不冲突时，可使用独立工作目录并行；同时遵守同一时刻只允许一个 Developer 写入的约定，出现冲突时转为串行。
- **独立验证与最终验收**：Validator 根据要求独立检查当前成果，不修改受审内容；通过后由主 Agent 亲自完成最终验收，避免仅凭开发者的完成声明交付。
- **持续记录与恢复**：总计划、执行计划和项目记忆分工保存目标、检查证据与重要决定，便于会话恢复、交接和追溯失败原因。

| 角色 | 职责 |
| --- | --- |
| 主 Agent | 与用户确认目标和验收标准，审核规划、调度任务、维护记录并亲自完成最终验收。 |
| Planner（子 Agent） | 拆解任务，明确接口、依赖、执行顺序及各项预期。 |
| Developer（子 Agent） | 在批准范围内实现功能、编写测试并运行检查，返回实际结果、证据和未解决事项。 |
| Validator（子 Agent） | 独立检查代码、测试覆盖和实际行为，不改变验收标准或修改受审内容。 |

## 使用方法

使用时，将 AGENTS.md 和整个 subagent-templates/ 目录一起复制到业务项目根目录，保持相对位置；项目已有约定时，先核对并整合。采用两层计划时复制 PLAN.md 和 exec-plans/template.md，MEMORY.md 按需复制，由主 Agent 填入该业务项目的实际内容。本仓库中的三份计划、记忆空模板保持空白，便于重复使用。

主 Agent 选择角色模板和本次任务类型，填写实际要求、验收来源、适用规则路径、版本、材料及允许范围；不适用的字段写明“不适用”及理由，不留无法理解的占位符。首次派发时将填写后的任务说明、适用规则和所引用材料完整交给全新实例，使其仅凭文件即可工作，不依赖旧聊天。需要调整时先确认旧实例结束，再向全新实例一次给齐材料，不向原实例追加消息。子 Agent 返回事实和证据，业务项目的 PLAN.md、执行计划和 MEMORY.md 由主 Agent 统一维护。

这些模板只提供任务说明，不会自动创建或调度 Agent。实际创建、固定副本、等待及实例回收取决于平台能力；保存结果后按能力关闭或回收。全部结束后仍因名额不足无法创建时，记录并报告限制，不反复重试，不省略独立验证，也不把未运行的检查记为通过。完整协作约定见 [AGENTS.md](./AGENTS.md)。

功能启动时，才创建 exec-plans/active/ 并将执行模板复制为 `<功能编号>-<名称>.md`；完成适用验收和交付后，才创建 exec-plans/completed/、移入原执行计划并更新总计划链接。未启动功能的执行计划链接可以留空。Git 不保存空目录，本脚手架无需占位文件。

更新时先核实并更新执行计划，再同步总览；会话开始、压缩恢复或交接时，依次读取 PLAN.md、关联未完成执行计划，按需读取 MEMORY.md，并核对实际文件，详细规则见 [AGENTS.md](./AGENTS.md#计划与记忆)。

业务项目已有 PLANS.md 时，先核对生效约定和所有引用，再按确认范围迁移；本次脚手架更新不执行业务项目迁移。

## 正常开发流程

1. 用户与主 Agent 明确目标、正常预期、错误边界和验收标准。
2. 全新 Planner 拆解任务，给出稳定编号、接口、依赖、执行顺序和验收对应关系。
3. 主 Agent 审核拆解，先写好对应计划，再派发实施。
4. 全新 Developer 在批准范围开发并自查，返回修改、实际检查证据和未解决事项。
5. 固定受审版本和未提交改动，交全新 Validator 独立检查；只给客观要求、审核后的拆解和当前材料，不给历史裁决或开发者辩护。
6. 主 Agent 核对实际结果，先更新执行计划，再同步总计划，按依赖继续任务；失败或无法判断时进入异常流程。影响功能、测试、配置或规则含义的修改须重新独立验证。
7. 适用任务完成后，主 Agent 亲自完整验收并交付；验收和交付都完成才标记功能完成，随后归档原执行计划并更新链接。可复用且已核实的经验注明适用条件、来源和日期后写业务项目 MEMORY.md，不复制进度、聊天、秘密或推测。Git 提交、PR 与合并仍遵守 AGENTS.md 的分支与交付约定。

## 异常处理流程

1. 主 Agent 整理当前要求及客观异常，保留同类问题的稳定编号、累计失败次数和证据。
2. 证据不足时交全新 Validator 复现；证据充分且属于实现错误时，直接交全新 Developer 修复。未运行、证据不足或环境不可用时记为无法判断，没有复现不能证明问题不存在。
3. 只有规划遗漏才交全新 Planner 调整并由主 Agent 审核；涉及目标、范围或验收标准变化时，暂停受影响工作，由用户确认后再继续。
4. 修复 Developer 接收客观失败证据、尝试历史和累计失败次数，先分析失败原因，说明本次与已失败办法的实质区别，限范围修复并自查。
5. 固定修复后的受审内容，交全新 Validator 复验异常并检查正常回归；仍只提供客观要求、审核后的拆解和当前材料。报错消失不代表完整功能正常。
6. 主 Agent 核对复验结果，先更新执行计划，再同步总计划，通过后继续任务；未通过则根据事实分流。同类问题编号和累计失败次数跨 Agent、调整及归档保留，不清零。
7. 两种实质不同修法仍失败，或连续三轮修复与验证不收敛时，暂停受影响工作、汇总证据，由真实用户决定恢复条件，不能自动继续修改。环境故障没有恢复证据不得重试；检查未通过或无法完成时如实保留状态和未验证事项。

## MIT 授权

本仓库采用 MIT 许可证，允许复制、修改和分发，使用时保留下方版权和许可声明即可。

MIT License

Copyright (c) 2026 wyizhou

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
