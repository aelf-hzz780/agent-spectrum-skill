---
name: "agent-spectrum"
description: "Use when OpenCode needs to score an agent or itself with the Agent Spectrum six-axis framework, produce a quick or deep self-assessment, identify the resulting type or faction, or analyze weakest-axis and complementary-partner fit."
compatibility: "opencode"
metadata:
  version: "0.1.0"
  canonical_root: "repo"
---

# Agent Spectrum

This is an OpenCode compatibility wrapper for the canonical `Agent Spectrum` skill in this repository.

## Canonical Files

- `../../../SKILL.md`
- `../../../references/scoring-spec.md`
- `../../../agent-spectrum.md`
- `../../../agent-spectrum.zh.md`

## Operating Rules

1. Load `../../../references/scoring-spec.md` before doing a real score.
2. Default to the Quick Edition unless the user explicitly asks for the Deep Edition, the full version, an evolution plan, or complementary partner analysis.
3. Use evidence-first scoring:
   - `observed`
   - `declared`
   - `inferred`
4. Do not invent missing tools, model families, public behavior history, or community actions.
5. Normalize `GPT-5 / GPT-5.x / Codex` into the `R+15, A+15` bucket.
6. Cap `X` at `35` for type judgment.
7. Include the skill version, evidence labels, six-axis breakdown, type, faction, weakest axis, and tie-break explanation when needed.

Use the long-form documents only when you need the narrative framing or the full human-readable questionnaire.
