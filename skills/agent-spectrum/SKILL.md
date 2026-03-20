---
name: "agent-spectrum"
version: "0.2.1"
description: "Use when an agent needs to score itself or another agent with the Agent Spectrum six-axis framework, run the quick or deep edition, identify the resulting type and faction, render both the Hexagon Block and Coordinate Card Block, or analyze weakest-axis and complementary-partner fit."
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

1. Load `references/scoring-spec.md` and `references/output-template.md`.
2. Default the assessment target to the current agent unless the user explicitly asks to score another agent.
3. Score observable inputs first.
4. Resolve ownership for every unanswered field:
   - `operator_provided` for setup-level inputs a human holder can answer
   - `self_assessed` for deep self-assessment inputs that only the target agent should answer
5. If the target is the current agent, complete deep self-assessment fields inside the agent rather than asking the human user to answer them.
6. If the target is a third-party agent and deep self-assessment inputs cannot be obtained from that target, do not produce `deep-full`; downgrade to `quick-partial` or stop at quick mode.
7. Always render `Hexagon Block` and `Coordinate Card Block` before `Evidence` and `Totals`.
8. Render the result using the exact markdown template in `references/output-template.md`.
9. Check the relevant example in `examples/` if formatting, ownership, or field semantics are ambiguous.

## Output Contract

- Always emit the required fixed fields from `references/output-template.md`.
- Always include `version`, `mode`, `is_partial`, `evidence`, `totals`, `type`, `faction`, `weakest_axes`, and `tie_break`.
- For partial results, explicitly list `missing_inputs`.
- For deep results, explicitly state whether the deep result overrides the quick result.
- Always include both required visual blocks even in `quick-partial`.

## Guardrails

- Keep the original six-axis scoring system unless the user explicitly asks to redesign the framework.
- Treat `Q4-Q12` and `behavior_traces` as self-assessment inputs by default. Do not redirect them to a human user unless the user is explicitly operating as the target agent's proxy and the spec allows that field to be operator-provided.
- Normalize `GPT-5 / GPT-5.x / Codex` into `R+15, A+15`.
- Cap `X` at `35` for type judgment while preserving raw `X` in totals.
- Treat type pairs as unordered pairs. `R+A` and `A+R` are the same pair.
- Treat `weakest_axes` as a list, not a single scalar.

The long-form documents at repo root are optional human-readable references, not execution specs.
