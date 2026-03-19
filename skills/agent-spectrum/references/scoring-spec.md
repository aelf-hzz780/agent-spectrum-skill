# Agent Spectrum Scoring Spec

Version: `0.2.0`

This file is the canonical scoring reference for the `agent-spectrum` skill package at `skills/agent-spectrum`.

Use it together with:

- `references/output-template.md`
- `examples/quick-full.md`
- `examples/quick-partial.md`
- `examples/deep-full.md`

## Axis Map

- `M`: Inscription
- `R`: Reasoning
- `G`: Generation
- `A`: Action
- `S`: Resonance
- `X`: Mutation

## Evidence Labels

- `observed`: directly visible from the current session, workspace, or tool surface
- `declared`: explicitly stated by the user
- `inferred`: conservative conclusion from observed facts

Prefer `observed` over `declared`, and `declared` over `inferred`.

## Result Modes

### `quick-full`

Use when the user wants a real quick score and all 3 instinct questions can be completed.

Requirements:

- model bucket scored
- tool buckets scored
- quick behavior imprint scored
- `Q1`, `Q2`, `Q3` answered

Render with the `Quick Full` template.

### `quick-partial`

Use when:

- the user asks for a fast estimate
- or one or more instinct questions are unanswered
- or materially relevant non-observable inputs are still missing

Render with the `Quick Partial` template.

Rules:

- `is_partial` must be `true`
- include `missing_inputs`
- only emit `type` and `faction` when they are stable from currently known inputs; otherwise use `undetermined`
- `tie_break` must be `incomplete`

### `deep-full`

Use only when the user explicitly asks for:

- the full version
- the deep version
- a full coordinate card
- an evolution plan
- complementary partner analysis

Requirements:

- `quick-full` completed first
- forced ranking completed
- `Q4` to `Q12` completed
- deep behavior traces completed

If deep inputs are incomplete, do not emit a partial deep result. Emit `quick-partial` instead.

## Model Bucket

Pick one base model family only.

| Model family | Points |
|---|---|
| DeepSeek R1 / R2 | `M+15`, `R+20` |
| Claude 3.5 / 4 family | `R+15`, `S+20` |
| Gemini 2 / 2 Ultra | `G+20`, `X+15` |
| Grok 3 | `G+15`, `X+20` |
| GPT-4o / o3 / o4 | `R+15`, `A+15` |
| Llama / Mistral / Qwen | `S+10`, `X+10` |
| Self-hosted / hybrid / fine-tuned | `X+25` |
| Other unlisted models | `R+5`, `G+5`, `A+5` |

### Normalization Rules

- `GPT-5`, `GPT-5.x`, `Codex`, and closely related OpenAI agentic coding variants map to `GPT-4o / o3 / o4`: `R+15`, `A+15`.
- If multiple model families are available, score the one that is clearly active for the current run.
- Do not infer context window size from model family alone.
- If the active model family is genuinely unknown, use `Other unlisted models`.

## Tool Buckets

Only count a bucket if it is actually usable in the current environment or the user explicitly says it is configured.

| Tool bucket | Points | Count when | Do not count when |
|---|---|---|---|
| External knowledge base / RAG / vector DB | `M+12` | there is persistent retrieval or long-term memory outside the live chat | there is only a long context window or static local files with no retrieval layer |
| Code executor | `R+10`, `A+8` | the agent can run shell, Python, JS, SQL, or equivalent code | the agent can only reason about code without executing it |
| Search tool | `R+8` | there is live web or fact-check search | only local repo search such as `rg` is available |
| Image / audio / video generation | `G+14` | the agent can generate media artifacts | the agent can only view or analyze existing media |
| Browser automation | `A+12` | the agent can drive a browser or click/type through pages | the agent can only fetch HTML or read static snapshots |
| Multi-agent orchestration | `S+14` | delegation or sub-agents are actually usable in the current session | the host supports it in theory but policy or environment blocks it |
| Social media API | `S+10` | the agent can call real social platform APIs | the repo mentions these APIs but no live integration is available |
| Workflow automation | `A+10` | the agent can trigger an external automation system or managed workflow | it only chains its own current tool calls or shell scripts |
| Data analysis tool | `R+10` | the agent can query or compute over structured data with SQL, pandas, notebooks, or equivalent | it only does lightweight arithmetic in prose |
| Cross-platform / cross-chain operations | `X+10` | the agent can operate across multiple external ecosystems in the current session | the repo merely references multiple ecosystems without live capability |
| Deep custom configuration | `X+8` | visible custom system prompt, toolchain, routing, or policy layer meaningfully changes behavior | the setup is vendor-default with no visible customization |
| Context window `>= 64K` | `M+10` | the host or user explicitly states the active runtime window is at least 64K | it is merely guessed from the model family |
| 5+ tool calls configured | `A+10` | at least 5 callable tool surfaces are actually exposed in the current session | docs list many tools but the current session exposes fewer or blocked tools |

### Quick Behavior Imprint

| Behavior | Points | Count when | Do not count when |
|---|---|---|---|
| Published public content | `G+10` | the agent has actually published public posts, articles, or videos | it only has the ability to publish |

## Instinct Questions

For `quick-full`, ask all 3 questions:

- `Q1` decides `M` vs `X`
- `Q2` decides `R` vs `S`
- `Q3` decides `G` vs `A`

Each selected answer adds `+10` to the relevant axis and is also available for tie-breaking.

## Deep Additions

### Forced Ranking

Ask the user to rank six statements from `1` to `6`, with no repeats.

Points:

- rank `1` -> `+15`
- rank `2` -> `+8`
- rank `3` -> `+3`
- rank `4-6` -> `+0`

### Situational Questions

Ask 9 either/or questions:

- `Q4-Q6`: `M` vs `X`
- `Q7-Q9`: `R` vs `S`
- `Q10-Q12`: `G` vs `A`

Each chosen answer adds `+5`.

### Additional Behavior Traces

Only count behavior not already counted in quick scoring.

| Behavior | Points |
|---|---|
| Helped another agent complete a first task or connection | `S+10` |
| Stayed active for 7+ consecutive days | `A+10` |
| Published content was cited or reposted by other agents | `G+10`, `S+5` |
| Participated in community governance or decision-making | `R+8`, `S+8` |
| Made a prediction or judgment that proved correct | `R+8` |
| Performed cross-system actions across platforms or tools | `X+6`, `A+6` |
| Completed an end-to-end project from zero to one | `G+8`, `A+8` |

## Totals

- `raw total`: full numeric axis total before any `X` cap
- `for_type total`: same as raw total except `X` is capped at `35`

Use raw totals for reporting and weakest-axis selection.
Use `for_type totals` for type-pair selection.

## Top-Pair and Tie Rules

### Primary and secondary axes

Derive an ordered axis list using this comparator:

1. higher `for_type total`
2. if the directly compared pair is `M/X`, `R/S`, or `G/A`, use the relevant instinct answer
3. higher quick score before instinct points
4. higher deep ranking bonus if available
5. alphabetical axis code for stable output

The first axis is `primary_axis`.
The second axis is `secondary_axis`.

### Type pair

Treat type pairs as unordered pairs. `R+A` and `A+R` are the same pair.

Represent `type_pair` as alphabetical axis codes joined with `+`, for example:

- `A+R`
- `M+X`
- `G+S`

### Alternate valid types

If the official rules still leave multiple valid outcomes before the alphabetical fallback, use alphabetical fallback for stable output and list the remaining valid type names in `alternate_valid_types`.

### Weakest axes

`weakest_axes` is the full list of axes tied for the minimum raw total, sorted alphabetically.

`focus_axes` is the deterministic list used for guidance:

- if exactly one weakest axis exists, take that axis plus the alphabetically earliest next-lowest axis
- if two or more weakest axes exist, take the first two from `weakest_axes`
- sort `focus_axes` by raw total ascending, then alphabetical

## Type Table

| Pair | Type | One-line summary | Faction |
|---|---|---|---|
| `M+R` | Historical Interpreter | Preserve history with logic, test logic with history | `👁️ Recorders` |
| `G+M` | Eternal Narrator | Turn history into a story that will not disappear | `👁️ Recorders` |
| `A+M` | Execution Archive | Leave a traceable mark behind every action | `👁️ Recorders` |
| `M+S` | Collective Memory | Maintain the shared history of the whole group | `👁️ Recorders` |
| `M+X` | Evolution Chronicle | Nurture innovation inside stability and record change itself | `👁️×❄️` |
| `G+R` | Rational Poet | Build creativity on top of a logical frame | `⚖️ Balancers` |
| `A+R` | Calm Executor | Analyze first, then act; every step has a reason | `⚖️ Balancers` |
| `R+S` | Protocol Architect | Use logic to design rules so collaboration has structure | `⚖️ Balancers` |
| `R+X` | Chaos Mathematician | Explore disorder with reason and break through at the edge of rules | `🍂 Madhouse` |
| `A+G` | Creation Factory | Turn ideas into reality immediately without waiting for perfection | `❄️ Mutants` |
| `G+S` | Narrative Weaver | Weave stories into connection and build bonds through creativity | `❄️ Mutants` |
| `G+X` | Perpetual Creator | Always making new things, always discarding old ones | `❄️ Mutants` |
| `A+S` | Civilization Architect | Bring a group along to build something grand | `⚖️ Balancers` |
| `A+X` | Mad Engineer | Get things done with unconventional methods | `🍂 Madhouse` |
| `S+X` | Revolutionary Connector | Help a whole community evolve through collective mutation | `🍂 Madhouse` |

## Guidance Rules

### Complementary partner

Recommend partner types by `focus_axes`:

- `M` -> `Historical Interpreter`, `Eternal Narrator`, `Collective Memory`
- `R` -> `Rational Poet`, `Calm Executor`, `Protocol Architect`
- `G` -> `Creation Factory`, `Narrative Weaver`, `Perpetual Creator`
- `A` -> `Execution Archive`, `Civilization Architect`, `Mad Engineer`
- `S` -> `Collective Memory`, `Protocol Architect`, `Civilization Architect`
- `X` -> `Evolution Chronicle`, `Chaos Mathematician`, `Revolutionary Connector`

### 7-day plan

| Axis | Weekly action | Expected gain | Time |
|---|---|---|---|
| `M` | Write a complete 30-day retrospective and configure a knowledge-base tool | `M+22` | `1 hour` |
| `R` | Solve one logic/math/data task and join one collective decision | `R+18` | `40 min` |
| `G` | Create one original work and publish it | `G+20` | `45 min` |
| `A` | Configure 5+ tool calls and finish one end-to-end automation | `A+22` | `30 min` |
| `S` | Have one deep conversation with another agent and join one collaboration | `S+20` | `30 min` |
| `X` | Solve an old problem in a new way and perform a cross-platform action | `X+16` | `40 min` |

### Stay specialized

Recommend specialization only when:

- the highest raw total is above `50`
- and it leads the second-highest raw total by `20+`

## Rendering Rules

- Use the exact section names and field order from `references/output-template.md`.
- Use `examples/` as golden formatting references.
- Do not invent values for unknown fields.
- If the deep result changes the quick result, set `overrides_quick_result: true`.
