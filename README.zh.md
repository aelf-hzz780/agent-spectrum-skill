[English](README.md) | [中文](README.zh.md)

# Agent Spectrum Skill

把 Agent Spectrum 六维框架封装成一个可复用的 Codex skill，用于 agent 自评分、类型判断和互补搭档分析。

## 概览

这个仓库把 `Agent Spectrum` 打包成一个 skill，让 agent 可以基于六个维度对自己或其他 agent 做分析：

- `M`：铭刻
- `R`：推演
- `G`：生成
- `A`：行动
- `S`：共振
- `X`：突变

这个 skill 支持：

- 快速版评分
- 深度版评分
- 类型与阵营判断
- 最空维度列表分析
- 互补伙伴建议
- 7 天进化建议

当前版本：`0.2.1`

## 仓库内容

- [`skills/agent-spectrum/SKILL.md`](skills/agent-spectrum/SKILL.md)：canonical skill package 入口
- [`skills/agent-spectrum/references/scoring-spec.md`](skills/agent-spectrum/references/scoring-spec.md)：canonical 评分规则
- [`skills/agent-spectrum/references/output-template.md`](skills/agent-spectrum/references/output-template.md)：严格 markdown 输出模板
- [`skills/agent-spectrum/examples/`](skills/agent-spectrum/examples)：golden examples
- [`agent-spectrum.md`](agent-spectrum.md)：英文长文阅读版
- [`agent-spectrum.zh.md`](agent-spectrum.zh.md)：中文源稿
- [`AGENTS.md`](AGENTS.md)：读取根目录说明的宿主可用的路由说明

## 宿主兼容层

这个仓库现在补了多宿主兼容入口：

- Codex / OpenAI：[`skills/agent-spectrum/`](skills/agent-spectrum)
- Claude Code：[`.claude/skills/agent-spectrum/SKILL.md`](.claude/skills/agent-spectrum/SKILL.md)
- OpenCode：[`.opencode/skills/agent-spectrum/SKILL.md`](.opencode/skills/agent-spectrum/SKILL.md)
- OpenClaw：[`skills/agent-spectrum/SKILL.md`](skills/agent-spectrum/SKILL.md)
- Cursor：[`AGENTS.md`](AGENTS.md) 和 [`.cursor/rules/agent-spectrum.mdc`](.cursor/rules/agent-spectrum.mdc)

canonical package 现在只放在 [`skills/agent-spectrum/`](skills/agent-spectrum)。其他宿主目录下的是轻量包装层，只负责把不同平台路由到同一套评分规则。

## 关键行为

- 默认走快速版，只有用户明确要求完整版或深度版时才继续。
- 采用 evidence-first 计分，使用 `observed`、`operator_provided`、`self_assessed`、`inferred` 标记依据。
- 使用固定 markdown 输出模板，而不是自由发挥格式。
- 始终输出 Hexagon Block 和 Coordinate Card Block 两种视觉块。
- deep situational answers 和 deep behavior traces 默认是 agent 自评字段。
- 保持原始六维语义和类型表不变。
- 将 `GPT-5 / GPT-5.x / Codex` 统一映射到 `GPT-4o / o3 / o4` 这一档，即 `R+15, A+15`。
- `X` 维用于类型判断时上限为 `35`。
- `weakest_axes` 是列表，类型判断按无序 pair 处理。

## 安装

如果本地通过 Codex / OpenAI skills 或 OpenClaw 使用，应安装 [`skills/agent-spectrum`](skills/agent-spectrum) 这个 canonical package，而不是 repo 根目录。

示例：

```bash
git clone <this-repo> /tmp/agent-spectrum-skill
mkdir -p "$CODEX_HOME/skills"
cp -R /tmp/agent-spectrum-skill/skills/agent-spectrum "$CODEX_HOME/skills/agent-spectrum"
```

对于 Claude Code、OpenCode、Cursor，这个仓库内的 wrapper 假设整个 repo 作为 workspace 存在。

## 使用示例

示例 prompt：

- `Use $agent-spectrum to score this agent in the Quick Edition.`
- `Use $agent-spectrum to run the full Deep Edition and explain the final type.`
- `Use $agent-spectrum to analyze complementary partner fit for this agent.`

## 仓库说明

- `agent-spectrum.zh.md` 是叙事文案和完整问卷结构的中文源稿。
- `agent-spectrum.md` 是与中文结构保持同步的英文阅读版。
- `skills/agent-spectrum/references/scoring-spec.md` 是 skill 实际执行时应优先加载的规则文档。
- `skills/agent-spectrum/references/output-template.md` 固定了跨宿主输出格式。
- `skills/agent-spectrum/examples/` 提供了回归和格式校验用的样例。

## License

本仓库采用 [MIT License](LICENSE)。
