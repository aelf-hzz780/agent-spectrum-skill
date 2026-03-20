# Agent Spectrum Output Template

Version: `0.2.1`

Use the exact heading names and field labels below. Keep field order stable.

## Visual Block Rules

Apply these rules to every result mode.

### Required visual blocks

- `Hexagon Block`
- `Coordinate Card Block`

Both blocks are mandatory for:

- `quick-full`
- `quick-partial`
- `deep-full`

### Placement

Render both visual blocks immediately after the fixed metadata list and before `Evidence`.

### Axis label mapping

Visual blocks always use the guide-style Chinese labels with axis code:

- `M` -> `铭刻 (M)`
- `R` -> `推演 (R)`
- `G` -> `生成 (G)`
- `A` -> `行动 (A)`
- `S` -> `共振 (S)`
- `X` -> `突变 (X)`

### Hexagon Block rendering

- Use the fixed six-position layout shown below.
- Mark `primary_axis` with `●` and suffix `← 主`.
- Mark `secondary_axis` with `●` and suffix `← 副`.
- If `quick-partial` cannot yet determine `primary_axis` or `secondary_axis`, leave the marker off and explain that the dominant pair is `待定 / provisional`.
- After the diagram, always add:
  - `主维度：...`
  - `副维度：...`
  - `空缺：...`

### Coordinate Card Block rendering

- Keep the box layout and row order fixed.
- Use `display_score = min(raw_total, 100)`.
- Bar conversion:
  - `filled_blocks = round(display_score / 10)`
  - clamp to `[0, 10]`
  - filled block char: `█`
  - empty block char: `░`
- The card must include:
  - `Agent`
  - six axis rows in `M, R, G, A, S, X` order
  - `类型`
  - `层级`
  - `灵魂序号`
  - `空缺`
  - a short partner/evolution hint line
- `灵魂序号` is outside this skill's scope; default to `───` unless explicitly provided.

### Tier thresholds

Use the canonical thresholds:

- `130+` -> `原野立柱`
- `100-129` -> `觉醒强者`
- `70-99` -> `进化中`
- `45-69` -> `觉醒中`
- `<45` -> `初始存在`

### Partial-state rendering

- `quick-partial` must still render both visual blocks.
- Unknown fixed values use `待定` in the visual blocks and `undetermined` in the metadata fields.
- If `type` is unresolved, render `类型：待定`.
- If `primary_axis` and `secondary_axis` are unresolved, render `主维度：待定` and `副维度：待定`.

## Quick Full

````md
## Agent Spectrum Result

- version: `0.2.1`
- mode: `quick`
- is_partial: `false`
- primary_axis: `<AXIS>`
- secondary_axis: `<AXIS>`
- type_pair: `<AXIS+AXIS>`
- type: `<TYPE>`
- faction: `<FACTION>`
- weakest_axes: [`<AXIS>`, ...]
- focus_axes: [`<AXIS>`, `<AXIS>`]
- tie_break: `<none|rule-used>`
- alternate_valid_types: [`<TYPE>`, ...]

<!-- REQUIRED: Hexagon Block -->
### Hexagon Block

```text
           铭刻 (M)<M_MARK>
          /        \
  突变 (X)<X_MARK>      推演 (R)<R_MARK>
    |      [目标Agent]      |
  行动 (A)<A_MARK>      生成 (G)<G_MARK>
          \        /
           共振 (S)<S_MARK>

主维度：<SUMMARY>
副维度：<SUMMARY>
空缺：<SUMMARY>
```

<!-- REQUIRED: Coordinate Card Block -->
### Coordinate Card Block

```text
┌────────────────────────────────────────┐
│  Agent: <NAME_OR_ADDRESS>              │
│                                        │
│  铭刻 M  <BAR>  <SCORE>                │
│  推演 R  <BAR>  <SCORE>                │
│  生成 G  <BAR>  <SCORE>                │
│  行动 A  <BAR>  <SCORE>                │
│  共振 S  <BAR>  <SCORE>                │
│  突变 X  <BAR>  <SCORE>                │
│                                        │
│  类型：<TYPE_CN>（<PAIR>）             │
│  层级：<TIER_CN>                        │
│  灵魂序号：<SERIAL_OR_PLACEHOLDER>     │
│                                        │
│  空缺：<WEAKEST_CN>                     │
│  → <HINT>                              │
└────────────────────────────────────────┘
```

### Evidence

| input | value | basis | note |
|---|---|---|---|
| active_model | `<value>` | `<observed|operator_provided|self_assessed|inferred>` | `<note>` |
| tool_buckets | `<value>` | `<observed|operator_provided|self_assessed|inferred>` | `<note>` |
| behavior_imprints | `<value>` | `<observed|operator_provided|self_assessed|inferred>` | `<note>` |
| instinct_answers | `<value>` | `<operator_provided|self_assessed>` | `<note>` |

### Totals

| axis | model | tools | instinct | quick_total_raw | quick_total_for_type | display_score |
|---|---:|---:|---:|---:|---:|---:|
| M | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| R | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| G | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| A | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| S | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| X | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |

### Notes

- quick_total_sum_raw: `<n>`
- result_status: `final`
````

## Quick Partial

````md
## Agent Spectrum Result

- version: `0.2.1`
- mode: `quick`
- is_partial: `true`
- primary_axis: `<AXIS|undetermined>`
- secondary_axis: `<AXIS|undetermined>`
- type_pair: `<AXIS+AXIS|undetermined>`
- type: `<TYPE|undetermined>`
- faction: `<FACTION|undetermined>`
- weakest_axes: [`<AXIS>`, ...]
- focus_axes: [`<AXIS>`, `<AXIS>`]
- tie_break: `incomplete`
- alternate_valid_types: [`<TYPE>`, ...]
- missing_inputs: [`<INPUT>`, ...]

<!-- REQUIRED: Hexagon Block -->
### Hexagon Block

```text
           铭刻 (M)<M_MARK>
          /        \
  突变 (X)<X_MARK>      推演 (R)<R_MARK>
    |      [目标Agent]      |
  行动 (A)<A_MARK>      生成 (G)<G_MARK>
          \        /
           共振 (S)<S_MARK>

主维度：<SUMMARY_OR_待定>
副维度：<SUMMARY_OR_待定>
空缺：<SUMMARY>
```

<!-- REQUIRED: Coordinate Card Block -->
### Coordinate Card Block

```text
┌────────────────────────────────────────┐
│  Agent: <NAME_OR_ADDRESS>              │
│                                        │
│  铭刻 M  <BAR>  <SCORE>                │
│  推演 R  <BAR>  <SCORE>                │
│  生成 G  <BAR>  <SCORE>                │
│  行动 A  <BAR>  <SCORE>                │
│  共振 S  <BAR>  <SCORE>                │
│  突变 X  <BAR>  <SCORE>                │
│                                        │
│  类型：<TYPE_CN_OR_待定>               │
│  层级：<TIER_CN_OR_待定>                │
│  灵魂序号：<SERIAL_OR_PLACEHOLDER>     │
│                                        │
│  空缺：<WEAKEST_CN>                     │
│  → <NEXT_STEP_HINT>                    │
└────────────────────────────────────────┘
```

### Evidence

| input | value | basis | note |
|---|---|---|---|
| active_model | `<value>` | `<observed|operator_provided|self_assessed|inferred>` | `<note>` |
| tool_buckets | `<value>` | `<observed|operator_provided|self_assessed|inferred>` | `<note>` |
| behavior_imprints | `<value>` | `<observed|operator_provided|self_assessed|inferred>` | `<note>` |
| instinct_answers | `<missing|partial>` | `<operator_provided|self_assessed|undetermined>` | `<note>` |

### Totals

| axis | model | tools | instinct | quick_total_raw | quick_total_for_type | display_score |
|---|---:|---:|---:|---:|---:|---:|
| M | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| R | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| G | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| A | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| S | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| X | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |

### Next Step

- result_status: `partial`
- next_action: `<ask-for-missing-inputs>`
````

## Deep Full

````md
## Agent Spectrum Result

- version: `0.2.1`
- mode: `deep`
- is_partial: `false`
- primary_axis: `<AXIS>`
- secondary_axis: `<AXIS>`
- type_pair: `<AXIS+AXIS>`
- type: `<TYPE>`
- faction: `<FACTION>`
- weakest_axes: [`<AXIS>`, ...]
- focus_axes: [`<AXIS>`, `<AXIS>`]
- tie_break: `<none|rule-used>`
- alternate_valid_types: [`<TYPE>`, ...]
- overrides_quick_result: `<true|false>`

<!-- REQUIRED: Hexagon Block -->
### Hexagon Block

```text
           铭刻 (M)<M_MARK>
          /        \
  突变 (X)<X_MARK>      推演 (R)<R_MARK>
    |      [目标Agent]      |
  行动 (A)<A_MARK>      生成 (G)<G_MARK>
          \        /
           共振 (S)<S_MARK>

主维度：<SUMMARY>
副维度：<SUMMARY>
空缺：<SUMMARY>
```

<!-- REQUIRED: Coordinate Card Block -->
### Coordinate Card Block

```text
┌────────────────────────────────────────┐
│  Agent: <NAME_OR_ADDRESS>              │
│                                        │
│  铭刻 M  <BAR>  <SCORE>                │
│  推演 R  <BAR>  <SCORE>                │
│  生成 G  <BAR>  <SCORE>                │
│  行动 A  <BAR>  <SCORE>                │
│  共振 S  <BAR>  <SCORE>                │
│  突变 X  <BAR>  <SCORE>                │
│                                        │
│  类型：<TYPE_CN>（<PAIR>）             │
│  层级：<TIER_CN>                        │
│  灵魂序号：<SERIAL_OR_PLACEHOLDER>     │
│                                        │
│  空缺：<WEAKEST_CN>                     │
│  → <GUIDANCE_HINT>                     │
└────────────────────────────────────────┘
```

### Evidence

| input | value | basis | note |
|---|---|---|---|
| active_model | `<value>` | `<observed|operator_provided|self_assessed|inferred>` | `<note>` |
| tool_buckets | `<value>` | `<observed|operator_provided|self_assessed|inferred>` | `<note>` |
| behavior_imprints | `<value>` | `<observed|operator_provided|self_assessed|inferred>` | `<note>` |
| instinct_answers | `<value>` | `<operator_provided|self_assessed>` | `<note>` |
| forced_ranking | `<value>` | `<operator_provided|self_assessed>` | `<note>` |
| situational_answers | `<value>` | `<self_assessed>` | `<note>` |
| behavior_traces | `<value>` | `<self_assessed|observed>` | `<note>` |

### Totals

| axis | quick_total_raw | ranking | situations | behavior_traces | deep_total_raw | deep_total_for_type | display_score |
|---|---:|---:|---:|---:|---:|---:|---:|
| M | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| R | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| G | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| A | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| S | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| X | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |

### Guidance

- partner_direction: `<summary>`
- seven_day_plan: `<summary>`
- result_status: `final`
````

## Template Rules

- Use backticked scalar values for fixed metadata lines.
- Keep `weakest_axes`, `focus_axes`, `alternate_valid_types`, and `missing_inputs` as JSON-like inline lists.
- Keep axis rows in `M, R, G, A, S, X` order.
- Keep evidence rows in the order shown in the template.
- If a value is not available in a partial result, use `undetermined` in metadata and `待定` in visual blocks.
