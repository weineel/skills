# Weineel Skills

这个仓库包含了 Weineel 实现的技能系统。技能是包含指令、脚本和资源的文件夹，agent 可以动态加载这些技能来增强能力。

## 技能列表

### vibe-spec

将用户需求转化为 vibe-kanban 任务规划和工作流。

**功能**：
- 分析用户需求，反复讨论确认技术方案和依赖
- 拆解成有依赖关系的任务列表
- 使用 vibe-kanban MCP 创建 issue 和 sub-issue
- 询问用户是否启动可并行的 vibe-kanban 任务

**触发方式**：当用户提到创建任务、拆分需求、vibe-kanban、任务管理等场景时自动触发。

### translator

翻译技能 - 可以指定目标语言，把后续输入的任意语言文本翻译成目标语言。

**触发方式**：当用户需要翻译文本或提到翻译相关需求时触发。

## 安装和使用

### skills 命令安装

```bash
pnpx skills install weineel/skills

pnpx skills install weineel/skills@vibe-spec
```

### Claude Code

1. 添加 marketplace

```bash
/plugin marketplace add weineel/skills
```

2. 添加插件到 Claude Code：

```bash
/plugin install all@weineel-skills
```

3. 使用技能：
   - 直接与 Claude 对话，当遇到匹配的场景时技能会自动触发

## 创建自己的技能

创建技能需要：

1. 在 `skills/` 目录下创建一个新文件夹
2. 添加 `SKILL.md` 文件，包含 YAML 前缀和技能指令
3. （可选）添加脚本、参考文档和资源文件

### SKILL.md 结构

```markdown
---
name: skill-name
description: 当 X 时使用此技能，用于做 Y
---

# 技能指令

在此处编写你的技能详细说明...
```

### 技能最佳实践

- **清晰的触发条件**：在 description 中明确说明何时使用该技能
- **结构化的输出**：定义清晰的输出格式和模板
- **循序渐进**：使用渐进式披露，将复杂内容放在参考文件中
- **可测试**：为技能功能编写测试用例

## 目录结构

```
skills/
├── .claude-plugin/
│   └── marketplace.json    # 插件配置文件
├── skills/
│   ├── vibe-spec/          # vibe-kanban 任务规划技能
│   │   ├── SKILL.md
│   │   └── evals/
│   │       └── evals.json
│   └── translator/         # 翻译技能
│       └── SKILL.md
├── commands/               # 自定义命令
└── README.md
```

## 开发和测试

### 运行技能测试

每个技能可以在 `evals/evals.json` 中包含测试用例。运行测试：

```bash
cd /path/to/skill
# 使用 skill-creator 技能来运行测试
```

### 评估技能

使用技能评估框架来衡量技能表现：
- 定量评估：通过率、执行时间、token 使用量
- 定性评估：用户反馈、输出质量

## 贡献

欢迎提交 Issue 和 Pull Request 来改进现有技能或添加新技能。

## 许可证

除非另有说明，此仓库中的技能基于 Apache 2.0 许可证开源。

## 资源

- [Anthropic Skills 文档](https://docs.anthropic.com/en/docs/agents-and-tools/skills-api)
- [Claude Code 文档](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code)
- [Vibe Kanban MCP](https://www.vibekanban.com/docs/integrations/vibe-kanban-mcp-server)
