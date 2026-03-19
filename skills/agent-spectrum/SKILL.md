---
name: "agent-spectrum"
version: "0.2.0"
description: "Use when an agent needs to score itself or another agent with the Agent Spectrum six-axis framework, run the quick or deep edition, identify the resulting type and faction, produce a strict markdown result card, or analyze weakest-axis and complementary-partner fit."
---

# Agent Spectrum

Use this directory as the canonical `Agent Spectrum` skill package.

## Canonical Files

- `references/scoring-spec.md`
- `references/output-template.md`
- `examples/quick-full.md`
- `examples/quick-partial.md`
- `examples/deep-full.md`

Do not rely on repo-root wrappers as the source of truth. Those wrappers should route here.

## Execution Order

1. Load `references/scoring-spec.md`.
2. Decide whether the result is `quick-full`, `quick-partial`, or `deep-full`.
3. Score only inputs that are `observed`, `declared`, or conservatively `inferred`.
4. Render the result using the exact markdown template in `references/output-template.md`.
5. Check the relevant example in `examples/` if formatting or field semantics are ambiguous.

## Output Contract

- Always emit the required fixed fields from `references/output-template.md`.
- Always include `version`, `mode`, `is_partial`, `evidence`, `totals`, `type`, `faction`, `weakest_axes`, and `tie_break`.
- For partial results, explicitly list `missing_inputs`.
- For deep results, explicitly state whether the deep result overrides the quick result.

## Guardrails

- Keep the original six-axis meaning and score values unless the user explicitly asks to redesign the framework.
- Normalize `GPT-5 / GPT-5.x / Codex` into `R+15, A+15`.
- Cap `X` at `35` for type judgment while preserving raw `X` in totals.
- Treat type pairs as unordered pairs. `R+A` and `A+R` are the same type pair.
- Treat `weakest_axes` as a list, not a single scalar.

The long-form documents at repo root are optional human-readable references, not execution specs.
