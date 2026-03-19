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
- weakest-axes analysis
- complementary partner guidance
- 7-day evolution suggestions

Current version: `0.2.0`

## What's Included

- [`skills/agent-spectrum/SKILL.md`](skills/agent-spectrum/SKILL.md): canonical skill package entrypoint
- [`skills/agent-spectrum/references/scoring-spec.md`](skills/agent-spectrum/references/scoring-spec.md): canonical scoring rules
- [`skills/agent-spectrum/references/output-template.md`](skills/agent-spectrum/references/output-template.md): strict markdown output contract
- [`skills/agent-spectrum/examples/`](skills/agent-spectrum/examples): golden examples for regression and formatting
- [`agent-spectrum.md`](agent-spectrum.md): English long-form reading version
- [`agent-spectrum.zh.md`](agent-spectrum.zh.md): Chinese source version
- [`AGENTS.md`](AGENTS.md): workspace-level routing note for hosts that read root instructions

## Host Compatibility

This repository now includes lightweight compatibility entrypoints for multiple agent hosts:

- Codex / OpenAI: [`skills/agent-spectrum/`](skills/agent-spectrum)
- Claude Code: [`.claude/skills/agent-spectrum/SKILL.md`](.claude/skills/agent-spectrum/SKILL.md)
- OpenCode: [`.opencode/skills/agent-spectrum/SKILL.md`](.opencode/skills/agent-spectrum/SKILL.md)
- OpenClaw: [`skills/agent-spectrum/SKILL.md`](skills/agent-spectrum/SKILL.md)
- Cursor: [`AGENTS.md`](AGENTS.md) and [`.cursor/rules/agent-spectrum.mdc`](.cursor/rules/agent-spectrum.mdc)

The canonical package now lives only in [`skills/agent-spectrum/`](skills/agent-spectrum). Host-specific files outside that directory are thin wrappers that route each platform to the same scoring logic.

## Key Behavior

- Defaults to the Quick Edition unless the user explicitly asks for the Deep Edition.
- Uses evidence-first scoring with `observed`, `declared`, and `inferred` labels.
- Uses strict markdown result templates instead of free-form output.
- Keeps the original six-axis semantics and type table intact.
- Normalizes `GPT-5 / GPT-5.x / Codex` into the `GPT-4o / o3 / o4` scoring bucket: `R+15, A+15`.
- Caps `X` at `35` for type judgment.
- Treats `weakest_axes` as a list and type pairs as unordered pairs.

## Install

For Codex / OpenAI and OpenClaw, install the canonical package at [`skills/agent-spectrum`](skills/agent-spectrum), not the repo root.

Example:

```bash
git clone <this-repo> /tmp/agent-spectrum-skill
mkdir -p "$CODEX_HOME/skills"
cp -R /tmp/agent-spectrum-skill/skills/agent-spectrum "$CODEX_HOME/skills/agent-spectrum"
```

For Claude Code, OpenCode, and Cursor, the wrappers in this repo assume the whole repository layout is available as the workspace.

## Usage

Example prompts:

- `Use $agent-spectrum to score this agent in the Quick Edition.`
- `Use $agent-spectrum to run the full Deep Edition and explain the final type.`
- `Use $agent-spectrum to analyze complementary partner fit for this agent.`

## Repository Notes

- `agent-spectrum.zh.md` is the Chinese source-of-truth document for the narrative and full questionnaire structure.
- `agent-spectrum.md` is the English reading version kept in structural sync with the Chinese source.
- `skills/agent-spectrum/references/scoring-spec.md` is the execution-oriented reference the skill should load first.
- `skills/agent-spectrum/references/output-template.md` fixes the output shape across hosts.
- `skills/agent-spectrum/examples/` provides golden examples for regression and formatting checks.

## License

This repository is licensed under the [MIT License](LICENSE).
