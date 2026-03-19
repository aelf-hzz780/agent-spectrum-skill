---
name: "agent-spectrum"
version: "0.1.0"
description: "Use when Codex needs to score an agent or itself with the Agent Spectrum six-axis framework, produce a quick or deep self-assessment, explain the resulting type, faction, weakest axis, and partner fit, or respond to requests that mention agent spectrum, six-axis coordinates, hexagon profile, quick edition, deep edition, or complementary-agent matching."
---

# Agent Spectrum

Use this skill to run the `Agent Spectrum` self-assessment in a way another agent can execute reliably.

## Skill Version

- Current skill version: `0.1.0`
- If two runs disagree, compare the reported version first before comparing the scores.

## Sources

- Chinese source of truth: `agent-spectrum.zh.md`
- English reading version: `agent-spectrum.md`
- Operational scoring reference: `references/scoring-spec.md`

Load `references/scoring-spec.md` for the actual workflow and scoring rules. Use the long-form documents only when you need the narrative framing, exact wording, or a human-readable card/template.

## Operating Rules

1. Default to the **Quick Edition** when the user asks for a score, type, coordinates, or a fast read.
2. Continue into the **Deep Edition** only when the user explicitly asks for the full version, deep version, evolution plan, or complementary partner analysis.
3. Use **evidence-first scoring**:
   - `observed`: visible from the current session, tool list, or workspace.
   - `declared`: explicitly stated by the user.
   - `inferred`: a narrow inference from observed facts. Use sparingly and label it.
4. Do not invent missing tools, model families, public behavior history, or community actions.
5. If a scoring item is not observable and materially affects the result, ask the user directly and keep the question focused.
6. Use the normalization rules in `references/scoring-spec.md` for modern model families, including the `GPT-5/Codex` mapping.

## Output Contract

For every scoring run, include:

- skill version
- mode used: `quick` or `deep`
- evidence labels for non-obvious scoring inputs
- six-axis score breakdown
- final type and faction
- weakest axis
- tie-break explanation when the top two axes are within 10 points

For the deep edition, also include:

- the completed deep totals
- complementary partner direction
- the 7-day evolution plan for the two weakest axes

## Guardrails

- Keep the original axis semantics and point values unless the user explicitly asks to redesign the framework.
- Cap `X` at `35` for type judgment, while preserving any overflow as a note if needed.
- Do not silently merge quick and deep results; state clearly when deep results override the quick result.
- Keep the answer concise even if the underlying scoring work is long.
