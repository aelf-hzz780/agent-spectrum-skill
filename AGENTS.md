# Agent Spectrum Workspace Instructions

This repository packages the `Agent Spectrum` framework for multiple agent hosts.

## Canonical Sources

- `skills/agent-spectrum/SKILL.md`: canonical skill entrypoint
- `skills/agent-spectrum/references/scoring-spec.md`: canonical scoring workflow
- `skills/agent-spectrum/references/output-template.md`: canonical output contract
- `skills/agent-spectrum/examples/`: golden examples
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

## Operating Rule

Load `skills/agent-spectrum/SKILL.md` and follow the canonical package exactly.
Do not re-derive the scoring logic from this `AGENTS.md`.

## Compatibility Layout

- Codex / OpenAI: `skills/agent-spectrum/`
- Claude Code: `.claude/skills/agent-spectrum/SKILL.md`
- OpenCode: `.opencode/skills/agent-spectrum/SKILL.md`
- OpenClaw: `skills/agent-spectrum/SKILL.md`
- Cursor: `.cursor/rules/agent-spectrum.mdc` and this root `AGENTS.md`
