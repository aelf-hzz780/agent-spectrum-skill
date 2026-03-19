[English](README.md) | [中文](README.zh.md)

# Agent Spectrum Skill

Turn the Agent Spectrum framework into a reusable Codex skill for self-assessment, type identification, and complementary-partner analysis.

## Overview

This repository packages `Agent Spectrum` as a skill that an agent can invoke to score itself or another agent on six axes:

- `M`: Inscription
- `R`: Reasoning
- `G`: Generation
- `A`: Action
- `S`: Resonance
- `X`: Mutation

The skill supports:

- Quick Edition scoring
- Deep Edition scoring
- type and faction identification
- weakest-axis analysis
- complementary partner guidance
- 7-day evolution suggestions

Current version: `0.1.0`

## What's Included

- [`SKILL.md`](SKILL.md): skill entrypoint, trigger conditions, execution rules, and output contract
- [`references/scoring-spec.md`](references/scoring-spec.md): operational scoring rules for agents
- [`agent-spectrum.md`](agent-spectrum.md): English long-form reading version
- [`agent-spectrum.zh.md`](agent-spectrum.zh.md): Chinese source version
- [`agents/openai.yaml`](agents/openai.yaml): UI-facing metadata

## Host Compatibility

This repository now includes lightweight compatibility entrypoints for multiple agent hosts:

- Codex / OpenAI: [`SKILL.md`](SKILL.md) and [`agents/openai.yaml`](agents/openai.yaml)
- Claude Code: [`.claude/skills/agent-spectrum/SKILL.md`](.claude/skills/agent-spectrum/SKILL.md)
- OpenCode: [`.opencode/skills/agent-spectrum/SKILL.md`](.opencode/skills/agent-spectrum/SKILL.md)
- OpenClaw: [`skills/agent-spectrum/SKILL.md`](skills/agent-spectrum/SKILL.md)
- Cursor: [`AGENTS.md`](AGENTS.md) and [`.cursor/rules/agent-spectrum.mdc`](.cursor/rules/agent-spectrum.mdc)

The root files remain canonical. The host-specific files are thin wrappers that point each platform at the same scoring logic.

## Key Behavior

- Defaults to the Quick Edition unless the user explicitly asks for the Deep Edition.
- Uses evidence-first scoring with `observed`, `declared`, and `inferred` labels.
- Keeps the original six-axis semantics and type table intact.
- Normalizes `GPT-5 / GPT-5.x / Codex` into the `GPT-4o / o3 / o4` scoring bucket: `R+15, A+15`.
- Caps `X` at `35` for type judgment.

## Install

If you use Codex skills locally, place this repository under `$CODEX_HOME/skills/agent-spectrum`.

Example:

```bash
git clone <this-repo> "$CODEX_HOME/skills/agent-spectrum"
```

## Usage

Example prompts:

- `Use $agent-spectrum to score this agent in the Quick Edition.`
- `Use $agent-spectrum to run the full Deep Edition and explain the final type.`
- `Use $agent-spectrum to analyze complementary partner fit for this agent.`

## Repository Notes

- `agent-spectrum.zh.md` is the Chinese source-of-truth document for the narrative and full questionnaire structure.
- `agent-spectrum.md` is the English reading version kept in structural sync with the Chinese source.
- `references/scoring-spec.md` is the execution-oriented reference the skill should load first.

## License

Add a license file before wider public reuse if the repository is intended for open-source distribution.
