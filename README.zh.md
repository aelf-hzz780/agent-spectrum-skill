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
- 最空维度分析
- 互补伙伴建议
- 7 天进化建议

当前版本：`0.1.0`

## 仓库内容

- [`SKILL.md`](SKILL.md)：skill 入口、触发条件、执行规则、输出约定
- [`references/scoring-spec.md`](references/scoring-spec.md)：面向 agent 的执行型评分规则
- [`agent-spectrum.md`](agent-spectrum.md)：英文长文阅读版
- [`agent-spectrum.zh.md`](agent-spectrum.zh.md)：中文源稿
- [`agents/openai.yaml`](agents/openai.yaml)：UI 元信息

## 宿主兼容层

这个仓库现在补了多宿主兼容入口：

- Codex / OpenAI：[`SKILL.md`](SKILL.md) 和 [`agents/openai.yaml`](agents/openai.yaml)
- Claude Code：[`.claude/skills/agent-spectrum/SKILL.md`](.claude/skills/agent-spectrum/SKILL.md)
- OpenCode：[`.opencode/skills/agent-spectrum/SKILL.md`](.opencode/skills/agent-spectrum/SKILL.md)
- OpenClaw：[`skills/agent-spectrum/SKILL.md`](skills/agent-spectrum/SKILL.md)
- Cursor：[`AGENTS.md`](AGENTS.md) 和 [`.cursor/rules/agent-spectrum.mdc`](.cursor/rules/agent-spectrum.mdc)

根目录文件仍然是 canonical source；各宿主目录下的是轻量包装层，目的只是把不同平台路由到同一套评分规则。

## 关键行为

- 默认走快速版，只有用户明确要求完整版或深度版时才继续。
- 采用 evidence-first 计分，使用 `observed`、`declared`、`inferred` 标记依据。
- 保持原始六维语义和类型表不变。
- 将 `GPT-5 / GPT-5.x / Codex` 统一映射到 `GPT-4o / o3 / o4` 这一档，即 `R+15, A+15`。
- `X` 维用于类型判断时上限为 `35`。

## 安装

如果本地通过 Codex skills 使用，可以把这个仓库放到 `$CODEX_HOME/skills/agent-spectrum`。

示例：

```bash
git clone <this-repo> "$CODEX_HOME/skills/agent-spectrum"
```

## 使用示例

示例 prompt：

- `Use $agent-spectrum to score this agent in the Quick Edition.`
- `Use $agent-spectrum to run the full Deep Edition and explain the final type.`
- `Use $agent-spectrum to analyze complementary partner fit for this agent.`

## 仓库说明

- `agent-spectrum.zh.md` 是叙事文案和完整问卷结构的中文源稿。
- `agent-spectrum.md` 是与中文结构保持同步的英文阅读版。
- `references/scoring-spec.md` 是 skill 实际执行时应优先加载的规则文档。

## License

如果要以开源仓库形式长期分发，建议补一个明确的 license 文件。
