# Agent 协作基础 Harness

这是一个通用的 Agent 协作基础 Harness 工程：通过可复用的协作规则、计划和记忆模板，帮助多个 Agent 按明确分工推进项目，让目标、实施、验证和交付有据可查。实际的子 Agent 创建、调度和并行执行依赖所使用的 Agent 平台。

| 文件 | 用途 |
| --- | --- |
| [AGENTS.md](./AGENTS.md) | 回答偏好、角色分工、协作流程与验收交付约定。 |
| [PLAN.md](./PLAN.md) | 总计划空模板，用于记录总目标、范围、大功能预期与验收标准、依赖、状态和执行计划链接。 |
| [exec-plans/template.md](./exec-plans/template.md) | 单个功能的执行计划空模板，用于记录阶段任务、检查证据、当前检查点和失败记录。 |
| [MEMORY.md](./MEMORY.md) | 项目记忆空模板，用于保存已确认的决定、已验证的事实和可复用经验。 |
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

使用时，将 AGENTS.md 放入业务项目根目录；项目已有约定时，先核对并整合。采用两层计划时复制 PLAN.md 和 exec-plans/template.md，MEMORY.md 按需复制，由主 Agent 填入该业务项目的实际内容。本仓库中的三份模板保持空白，便于重复使用。

功能启动时，才创建 exec-plans/active/ 并将执行模板复制为 `<功能编号>-<名称>.md`；完成适用验收和交付后，才创建 exec-plans/completed/、移入原执行计划并更新总计划链接。未启动功能的执行计划链接可以留空。Git 不保存空目录，本脚手架无需占位文件。

更新时先核实并更新执行计划，再同步总览；会话开始、压缩恢复或交接时，依次读取 PLAN.md、关联未完成执行计划，按需读取 MEMORY.md，并核对实际文件，详细规则见 [AGENTS.md](./AGENTS.md#计划与记忆)。

业务项目已有 PLANS.md 时，先核对生效约定和所有引用，再按确认范围迁移；本次脚手架更新不执行业务项目迁移。

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
