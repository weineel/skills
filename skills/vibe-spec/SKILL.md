---
name: vibe-spec
description: 将用户需求转化为 vibe-kanban 任务规划和工作流。当用户提到创建任务、拆分需求、vibe-kanban、任务管理、项目规划时使用此技能。帮助用户分析需求、确认技术方案、拆解带依赖关系的任务列表，并使用 vibe-kanban MCP 创建 issue 和启动 workspace。
---

# Vibe-Spec 技能

将用户需求转化为结构化的 vibe-kanban 任务规划。

## 核心工作流

### 1. 需求分析与技术讨论

首先，与用户深入讨论需求，确保理解完整：

- **需求澄清**: 用户想要实现什么功能？解决什么问题？
- **技术方案**: 采用什么技术栈/架构？有什么设计约束？
- **依赖关系**: 有哪些外部依赖？内部模块间的依赖如何？
- **验收标准**: 怎么算完成？有什么测试要求？

**重要**: 不要急于创建任务。先确保你和用户对需求的理解一致。如果需求模糊，主动提问澄清。

### 2. 任务拆解

将需求拆解为有依赖关系的任务列表：

- 每个任务应该是**可独立执行**的工作单元
- 明确任务之间的**前后依赖关系**（哪些任务必须先完成）
- 为每个任务定义清晰的**验收标准**
- 考虑是否需要**子 issue**（sub-issue）来表示更细粒度的工作

任务拆解原则：
- **原子性**: 每个任务应该能在一个 session 内完成
- **独立性**: 任务之间耦合尽可能小
- **可测试**: 每个任务完成后能验证是否成功
- **依赖清晰**: 明确哪些任务必须先于其他任务完成

### 3. 创建 Issue

使用 vibe-kanban MCP 的 `create_issue` 工具创建任务：

1. **获取当前 workspace 信息**: 调用 `list_workspaces` 或 `get_context` 获取当前 linked project
2. **创建父 issue**（如果有）: 代表整个功能/需求
3. **创建子 issue**: 每个拆解的任务作为子 issue，设置 `parent_issue_id`
4. **建立依赖关系**: 使用 `create_issue_relationship` 设置 `blocking` 关系

创建 issue 时的字段：
- `title`: 任务标题（简洁描述要做什么）
- `description`: 任务详情（包含技术方案、验收标准）
- `priority`: 优先级 (`urgent`/`high`/`medium`/`low`)
- `parent_issue_id`: 父 issue ID（如果是子任务）

### 4. 询问是否启动 Workspace

创建完所有 issue 后，主动询问用户：

> 任务已创建完成。是否现在启动 vibe-kanban workspace 来执行这些任务？

如果用户确认启动：

1. **获取仓库信息**: 调用 `list_repos` 获取可用仓库
2. **启动 workspace**: 调用 `start_workspace`
   - `name`: 任务名称
   - `executor`: 默认使用 `CLAUDE_CODE`
   - `repositories`: 用户选择的仓库列表
   - `issue_id`: 关联到主 issue（可选）

## 使用 vibe-kanban MCP 的工具清单

以下是你需要的核心工具：

| 工具 | 用途 |
|------|------|
| `list_workspaces` | 获取当前 workspace 和关联的 project |
| `list_repos` | 获取可用仓库列表 |
| `create_issue` | 创建 issue/sub-issue |
| `create_issue_relationship` | 建立任务间的 blocking 依赖关系 |
| `start_workspace` | 启动执行任务的 workspace |
| `link_workspace_issue` | 将 workspace 关联到 issue |

## 示例交互

**用户**: 我想用 vibe-kanban 管理这个新功能开发

**你**:
1. 先了解需求细节
2. 讨论技术方案
3. 拆解任务列表
4. 创建 issue
5. 询问是否启动 workspace

---

## 注意事项

- **不要跳过讨论环节**: 确保用户确认技术方案后再创建任务
- **依赖关系很重要**: 使用 `blocking` 关系明确任务先后顺序
- **任务粒度**: 不要太粗（无法在一个 session 完成）也不要太细（过度拆分）
- **灵活调整**: 如果用户想跳过某些步骤，尊重用户的选择
