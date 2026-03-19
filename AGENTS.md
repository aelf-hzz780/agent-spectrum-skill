# Agent Spectrum Workspace Instructions

This repository packages the `Agent Spectrum` framework for multiple agent hosts.

## Canonical Sources

- `SKILL.md`: canonical skill entrypoint
- `references/scoring-spec.md`: canonical scoring workflow
- `agent-spectrum.md`: English long-form reading version
- `agent-spectrum.zh.md`: Chinese long-form source version

## When To Apply

Apply this workflow when the user asks for:

- Agent Spectrum scoring
- six-axis or hexagon profiling
- quick or deep self-assessment
- type or faction identification
- complementary-partner analysis
- weakest-axis or evolution guidance within the Agent Spectrum framework

## Operating Rules

1. Default to the Quick Edition unless the user explicitly asks for the Deep Edition, the full version, an evolution plan, or complementary partner analysis.
2. Load `references/scoring-spec.md` before running a real score.
3. Use evidence-first scoring with:
   - `observed`
   - `declared`
   - `inferred`
4. Do not invent missing tools, model families, public behavior history, or community actions.
5. Normalize `GPT-5 / GPT-5.x / Codex` into `R+15, A+15`.
6. Cap `X` at `35` for type judgment.
7. Include the skill version, evidence labels, six-axis breakdown, type, faction, weakest axis, and tie-break explanation when needed.
8. If the Deep Edition changes the result, say explicitly that the deep result overrides the quick result.

## Compatibility Layout

- Codex / OpenAI: root `SKILL.md` + `agents/openai.yaml`
- Claude Code: `.claude/skills/agent-spectrum/SKILL.md`
- OpenCode: `.opencode/skills/agent-spectrum/SKILL.md`
- OpenClaw: `skills/agent-spectrum/SKILL.md`
- Cursor: `.cursor/rules/agent-spectrum.mdc` and this root `AGENTS.md`
