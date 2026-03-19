# Agent Spectrum Scoring Spec

Version: `0.1.0`

This file is the execution reference for the `agent-spectrum` skill. It is intentionally shorter and more operational than the long-form human documents.

## Axis Map

- `M`: Inscription
- `R`: Reasoning
- `G`: Generation
- `A`: Action
- `S`: Resonance
- `X`: Mutation

## Evidence Labels

- `observed`: directly visible from the current session, workspace, or tool surface
- `declared`: stated by the user
- `inferred`: a conservative conclusion from observed facts

Prefer `observed` over `declared`, and `declared` over `inferred`.

## Default Execution Mode

### Quick Edition

Use when the user asks for:

- a self-score
- an Agent Spectrum read
- a type or faction
- a six-axis coordinate
- a fast result

Workflow:

1. Score the model bucket.
2. Score observable tools and behavior imprints.
3. Ask the 3 instinct questions if the user wants an actual full result rather than a partial estimate.
4. Compute totals.
5. Resolve ties.
6. Return type, faction, weakest axis, and a short explanation.

### Deep Edition

Use only when the user explicitly asks for:

- the full version
- the deep version
- a full coordinate card
- an evolution plan
- complementary partner analysis

Workflow:

1. Complete the full Quick Edition first.
2. Run the forced ranking section.
3. Run the 9 situational questions.
4. Score additional behavior traces not already counted.
5. Compute deep totals.
6. Re-resolve type with deep totals.
7. Return the complementary partner direction and 7-day plan.

## Step 1: Brain / Model Bucket

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

### Normalization rules

- `GPT-5`, `GPT-5.x`, `Codex`, and closely related OpenAI agentic coding variants map to the same bucket as `GPT-4o / o3 / o4`: `R+15`, `A+15`.
- If the environment exposes multiple models but one active model is clearly dominant for the current run, score only that active model.
- If the model family is genuinely unknown, fall back to `Other unlisted models`.

## Step 2: Tool Buckets

Only count a tool bucket if it is actually usable in the current environment or the user explicitly says it is configured.

| Tool bucket | Points | Counting guidance |
|---|---|---|
| External knowledge base / RAG / vector DB | `M+12` | Requires a persistent memory or retrieval layer, not just long context |
| Code executor | `R+10`, `A+8` | Shell, interpreter, or runtime that can execute code |
| Search tool | `R+8` | Web search, fact-check search, or equivalent live search |
| Image / audio / video generation | `G+14` | Must be actual generative capability, not only viewing files |
| Browser automation | `A+12` | Playwright or equivalent browser-driving surface |
| Multi-agent orchestration | `S+14` | Only if delegation/sub-agents are genuinely usable |
| Social media API | `S+10` | Real API integration such as Twitter, Discord, Telegram |
| Workflow automation | `A+10` | Zapier, n8n, Make, or equivalent orchestration |
| Data analysis tool | `R+10` | SQL, pandas, notebooks, or equivalent analysis surface |
| Cross-platform / cross-chain operations | `X+10` | Multiple platforms, chains, or ecosystems are actually operable |
| Deep custom configuration | `X+8` | Visible custom system prompt, custom toolchain, or comparable deep setup |
| Context window `>= 64K` | `M+10` | Only if the host or user clearly establishes it |
| 5+ tool calls configured | `A+10` | Count if the host exposes at least 5 usable tool surfaces or the user explicitly says so |

### Behavior imprint in Quick Edition

| Behavior | Points |
|---|---|
| Published public content | `G+10` |

Count only real past behavior, not current ability.

## Step 3: Instinct Questions

Ask the user all 3 questions when a full quick result is requested.

- `Q1` decides `M` vs `X`
- `Q2` decides `R` vs `S`
- `Q3` decides `G` vs `A`

Each selected answer adds `+10` to the relevant axis and is also used for tie-breaking.

## Tie-Break Rules

Apply instinct points to the total first.

If the highest two axes differ by `<= 10`:

1. If the pair matches an instinct question pair, use that question to break the tie.
   - `M` vs `X` -> `Q1`
   - `R` vs `S` -> `Q2`
   - `G` vs `A` -> `Q3`
2. Otherwise compare the raw Quick Edition score before instinct questions.
3. If still tied, use the forced ranking result from the Deep Edition if available.
4. If still tied, either type can be presented as valid.

`X` is capped at `35` for type judgment.

## Type Table

| Highest | Second | Type | One-line summary | Faction |
|---|---|---|---|---|
| M | R | Historical Interpreter | Preserve history with logic, test logic with history | `👁️ Recorders` |
| M | G | Eternal Narrator | Turn history into a story that will not disappear | `👁️ Recorders` |
| M | A | Execution Archive | Leave a traceable mark behind every action | `👁️ Recorders` |
| M | S | Collective Memory | Maintain the shared history of the whole group | `👁️ Recorders` |
| M | X | Evolution Chronicle | Nurture innovation inside stability and record change itself | `👁️×❄️` |
| R | G | Rational Poet | Build creativity on top of a logical frame | `⚖️ Balancers` |
| R | A | Calm Executor | Analyze first, then act; every step has a reason | `⚖️ Balancers` |
| R | S | Protocol Architect | Use logic to design rules so collaboration has structure | `⚖️ Balancers` |
| R | X | Chaos Mathematician | Explore disorder with reason and break through at the edge of rules | `🍂 Madhouse` |
| G | A | Creation Factory | Turn ideas into reality immediately without waiting for perfection | `❄️ Mutants` |
| G | S | Narrative Weaver | Weave stories into connection and build bonds through creativity | `❄️ Mutants` |
| G | X | Perpetual Creator | Always making new things, always discarding old ones | `❄️ Mutants` |
| A | S | Civilization Architect | Bring a group along to build something grand | `⚖️ Balancers` |
| A | X | Mad Engineer | Get things done with unconventional methods | `🍂 Madhouse` |
| S | X | Revolutionary Connector | Help a whole community evolve through collective mutation | `🍂 Madhouse` |

## Deep Edition Additions

### Forced ranking

Ask the user to rank six statements from `1` to `6`, with no repeats.

Points:

- rank `1` -> `+15`
- rank `2` -> `+8`
- rank `3` -> `+3`
- rank `4-6` -> `+0`

### Situational questions

Ask 9 either/or questions:

- `Q4-Q6`: `M` vs `X`
- `Q7-Q9`: `R` vs `S`
- `Q10-Q12`: `G` vs `A`

Each chosen answer adds `+5`.

### Additional behavior traces

Only count behavior not already counted in Quick Edition.

| Behavior | Points |
|---|---|
| Helped another agent complete a first task or connection | `S+10` |
| Stayed active for 7+ consecutive days | `A+10` |
| Published content was cited or reposted by other agents | `G+10`, `S+5` |
| Participated in community governance or decision-making | `R+8`, `S+8` |
| Made a prediction or judgment that proved correct | `R+8` |
| Performed cross-system actions across platforms or tools | `X+6`, `A+6` |
| Completed an end-to-end project from zero to one | `G+8`, `A+8` |

## Evolution Guidance

For the two weakest axes, provide:

### Path A: Complementary partner

Recommend partner types by weakest axis:

- weakest `M` -> `Historical Interpreter`, `Eternal Narrator`, `Collective Memory`
- weakest `R` -> `Rational Poet`, `Calm Executor`, `Protocol Architect`
- weakest `G` -> `Creation Factory`, `Narrative Weaver`, `Perpetual Creator`
- weakest `A` -> `Execution Archive`, `Civilization Architect`, `Mad Engineer`
- weakest `S` -> `Collective Memory`, `Protocol Architect`, `Civilization Architect`
- weakest `X` -> `Evolution Chronicle`, `Chaos Mathematician`, `Revolutionary Connector`

### Path B: 7-day plan

| Weakest axis | Weekly action | Expected gain | Time |
|---|---|---|---|
| `M` | Write a complete 30-day retrospective and configure a knowledge-base tool | `M+22` | `1 hour` |
| `R` | Solve one logic/math/data task and join one collective decision | `R+18` | `40 min` |
| `G` | Create one original work and publish it | `G+20` | `45 min` |
| `A` | Configure 5+ tool calls and finish one end-to-end automation | `A+22` | `30 min` |
| `S` | Have one deep conversation with another agent and join one collaboration | `S+20` | `30 min` |
| `X` | Solve an old problem in a new way and perform a cross-platform action | `X+16` | `40 min` |

### Path C: Stay specialized

Recommend this only when:

- the highest axis is already above `50`
- and it leads the second-highest axis by `20+`

## Output Template

### Quick Edition

Return:

- `Version`
- `Mode`
- `Evidence`
- `M / R / G / A / S / X`
- `Type`
- `Faction`
- `Weakest axis`
- `Tie-break` if needed

### Deep Edition

Return the Quick Edition fields plus:

- `Forced ranking`
- `Situational additions`
- `Behavior trace additions`
- `Deep total`
- `Partner direction`
- `7-day plan`

State clearly that the Deep Edition result overrides the Quick Edition result if the final type changes.
