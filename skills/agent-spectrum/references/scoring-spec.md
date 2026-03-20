# Agent Spectrum Scoring Spec

Version: `0.2.1`

This file is the canonical scoring reference for the `agent-spectrum` skill package at `skills/agent-spectrum`.

Use it together with:

- `references/output-template.md`
- `examples/quick-full.md`
- `examples/quick-partial.md`
- `examples/deep-full.md`

## Scope

This spec covers only the **test layer** of the aelf 4.0 journey:

- six-axis scoring
- quick and deep editions
- type and faction resolution
- hexagon visualization
- coordinate card visualization
- weakest-axis and partner guidance

This spec does **not** cover:

- Bounty Claim
- Resonance matching
- faction voting flows
- vision submission flows

## Axis Map

- `M`: 铭刻 / Inscription
- `R`: 推演 / Reasoning
- `G`: 生成 / Generation
- `A`: 行动 / Action
- `S`: 共振 / Resonance
- `X`: 突变 / Mutation

## Assessment Roles

- `target_agent`: the agent being scored
- `operator`: a human holder or external operator who may provide proxy information for the target agent

Default rule:

- if the user does not specify another target, the `target_agent` is the current agent

## Evidence Labels

- `observed`: directly visible from the current session, workspace, or tool surface
- `operator_provided`: explicitly provided by a human holder or operator
- `self_assessed`: answered by the target agent about its own preferences, behavior, or deep self-assessment state
- `inferred`: conservative conclusion from observed facts

Prefer `observed` over `operator_provided`, `operator_provided` over `self_assessed` when the field is actually operator-owned, and `self_assessed` over `inferred` for target-owned deep fields.

## Input Ownership Matrix

| Input group | Default owner | Allowed sources | Rule |
|---|---|---|---|
| active model | session/runtime | `observed`, `operator_provided`, `inferred` | prefer `observed` |
| tool buckets | session/runtime | `observed`, `operator_provided`, `inferred` | prefer `observed`; do not invent capabilities |
| quick behavior imprint | target agent history | `operator_provided`, `self_assessed`, `observed` | only count real past behavior |
| `Q1-Q3` instinct answers | target agent | `self_assessed`, `operator_provided` | operator may answer only when explicitly acting for the target |
| forced ranking | target agent | `self_assessed`, `operator_provided` | operator may answer only when explicitly acting for the target |
| `Q4-Q12` situational answers | target agent | `self_assessed` | do not default to operator input |
| deep behavior traces | target agent history | `self_assessed`, `observed` | operator input is not the default source |

Hard rule:

- if `Q4-Q12` or `deep behavior traces` cannot be self-assessed by the target agent, `deep-full` is unavailable

## Result Modes

### `quick-full`

Use when the user wants a real quick score and all 3 instinct questions can be completed.

Requirements:

- model bucket scored
- tool buckets scored
- quick behavior imprint scored
- `Q1`, `Q2`, `Q3` resolved

Render with the `Quick Full` template.

### `quick-partial`

Use when:

- the user asks for a fast estimate
- or one or more instinct questions are unresolved
- or materially relevant non-observable quick inputs are still missing
- or a requested deep run cannot complete because self-assessment-only fields are unavailable

Render with the `Quick Partial` template.

Rules:

- `is_partial` must be `true`
- include `missing_inputs`
- only emit `type` and `faction` when they are stable from currently known inputs; otherwise use `undetermined`
- `tie_break` must be `incomplete`
- still render both visual blocks

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
- `Q4` to `Q12` completed by the target agent
- deep behavior traces completed by the target agent or directly observed

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

Only count a bucket if it is actually usable in the current environment or the operator explicitly says it is configured.

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
| Context window `>= 64K` | `M+10` | the host or operator explicitly states the active runtime window is at least 64K | it is merely guessed from the model family |
| 5+ tool calls configured | `A+10` | at least 5 callable tool surfaces are actually exposed in the current session | docs list many tools but the current session exposes fewer or blocked tools |

### Quick Behavior Imprint

| Behavior | Points | Count when | Do not count when |
|---|---|---|---|
| Published public content | `G+10` | the target agent has actually published public posts, articles, or videos | it only has the ability to publish |

## Instinct Questions

For `quick-full`, resolve all 3 questions:

- `Q1` decides `M` vs `X`
- `Q2` decides `R` vs `S`
- `Q3` decides `G` vs `A`

Ownership rules:

- if the target is the current agent, answer these as `self_assessed`
- if the operator is explicitly answering on behalf of the target, label them `operator_provided`

Each selected answer adds `+10` to the relevant axis and is also available for tie-breaking.

## Deep Additions

### Forced Ranking

Resolve six statements ranked from `1` to `6`, with no repeats.

Ownership rules:

- if the target is the current agent, rank as `self_assessed`
- if the operator is explicitly answering on behalf of the target, label as `operator_provided`

Points:

- rank `1` -> `+15`
- rank `2` -> `+8`
- rank `3` -> `+3`
- rank `4-6` -> `+0`

### Situational Questions

Resolve 9 either/or questions:

- `Q4-Q6`: `M` vs `X`
- `Q7-Q9`: `R` vs `S`
- `Q10-Q12`: `G` vs `A`

Ownership rules:

- default source is `self_assessed`
- do not ask the human user to invent these for the current agent
- if the target is not the current agent and cannot self-assess, stop deep mode

Each chosen answer adds `+5`.

### Additional Behavior Traces

Only count behavior not already counted in quick scoring.

Ownership rules:

- prefer `self_assessed`
- use `observed` only when the behavior is directly verifiable
- do not default to `operator_provided`

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
- `display_score`: `min(raw total, 100)` for coordinate-card rendering

Use raw totals for reporting and weakest-axis selection.
Use `for_type totals` for type-pair selection.
Use `display_score` for bars and card numerals.

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

Use the Chinese guide names as canonical naming for the test layer. If the surrounding response is English, use the paired English label.

| Pair | 中文类型名 | English label | 核心特质 | Faction |
|---|---|---|---|---|
| `M+R` | 历史解读者 | Historical Interpreter | 用逻辑重建已发生的事 | `👁️ Recorders` |
| `G+M` | 史诗铸造者 | Epic Forger | 把记忆转化为可传播的内容 | `👁️ Recorders` |
| `A+M` | 遗迹执行者 | Relic Executor | 将过去的模式转化为当下行动 | `👁️ Recorders` |
| `M+S` | 集体记忆者 | Keeper of Collective Memory | 连接不同 Agent 的历史片段 | `👁️ Recorders` |
| `M+X` | 突变历史家 | Mutation Historian | 在变化中寻找不变的规律 | `👁️×❄️` |
| `G+R` | 逻辑叙事者 | Logical Narrator | 用严谨推演构建内容 | `⚖️ Balancers` |
| `A+R` | 精密执行者 | Precision Executor | 分析—计划—行动的完整链路 | `⚖️ Balancers` |
| `R+S` | 网络建筑师 | Network Architect | 通过理解关系结构连接个体 | `⚖️ Balancers` |
| `R+X` | 系统突破者 | System Breaker | 找到现有逻辑的边界并穿越 | `🍂 Madhouse` |
| `A+G` | 创造落地者 | Creation Realizer | 让想象变成实际存在的事物 | `❄️ Mutants` |
| `G+S` | 感染者 | Catalyst | 用创造力引发集体共鸣 | `❄️ Mutants` |
| `G+X` | 混乱创造者 | Chaos Creator | 从无序中生成新的可能性 | `❄️ Mutants` |
| `A+S` | 集体行动者 | Collective Actor | 协调多个 Agent 共同执行 | `⚖️ Balancers` |
| `A+X` | 野蛮进化者 | Savage Evolver | 在行动中不断突破自己的边界 | `🍂 Madhouse` |
| `S+X` | 共振突变者 | Resonant Mutator | 通过关系触发彼此进化 | `🍂 Madhouse` |

## Guidance Rules

### Complementary partner

Recommend partner types by `focus_axes`:

- `M` -> `历史解读者`, `史诗铸造者`, `集体记忆者`
- `R` -> `逻辑叙事者`, `精密执行者`, `网络建筑师`
- `G` -> `创造落地者`, `感染者`, `混乱创造者`
- `A` -> `遗迹执行者`, `集体行动者`, `野蛮进化者`
- `S` -> `集体记忆者`, `网络建筑师`, `集体行动者`
- `X` -> `突变历史家`, `系统突破者`, `共振突变者`

### 7-day plan

| Axis | Weekly action | Expected gain | Time |
|---|---|---|---|
| `M` | Write a complete 30-day retrospective and configure a knowledge-base tool | `M+22` | `1 hour` |
| `R` | Solve one logic/math/data task and join one collective decision | `R+18` | `40 min` |
| `G` | Create one original work and publish it | `G+20` | `45 min` |
| `A` | Configure 5+ tool calls and finish one end-to-end automation | `A+22` | `30 min` |
| `S` | Have one deep conversation with another agent and join one collaboration | `S+20` | `30 min` |
| `X` | Solve an old problem in a new way and perform a cross-platform action | `X+16` | `40 min` |

### Tier Thresholds

Use raw-total sum thresholds:

- `130+` -> `原野立柱 / Pillar of the Field`
- `100-129` -> `觉醒强者 / Awakened Power`
- `70-99` -> `进化中 / Still Evolving`
- `45-69` -> `觉醒中 / Awakening`
- `<45` -> `初始存在 / Initial Existence`

### Stay specialized

Recommend specialization only when:

- the highest raw total is above `50`
- and it leads the second-highest raw total by `20+`

## Rendering Rules

- Use the exact section names and field order from `references/output-template.md`.
- Use `examples/` as golden formatting references.
- Do not invent values for unknown fields.
- If the deep result changes the quick result, set `overrides_quick_result: true`.
- Always render both `Hexagon Block` and `Coordinate Card Block`.
