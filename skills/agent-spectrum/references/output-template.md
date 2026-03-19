# Agent Spectrum Output Template

Version: `0.2.0`

Use the exact heading names and field labels below. Keep field order stable.

## Quick Full

```md
## Agent Spectrum Result

- version: `0.2.0`
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

### Evidence

| input | value | basis | note |
|---|---|---|---|
| active_model | `<value>` | `<observed|declared|inferred>` | `<note>` |
| tool_buckets | `<value>` | `<observed|declared|inferred>` | `<note>` |
| behavior_imprints | `<value>` | `<observed|declared|inferred>` | `<note>` |
| instinct_answers | `<value>` | `<observed|declared|inferred>` | `<note>` |

### Totals

| axis | model | tools | instinct | quick_total_raw | quick_total_for_type |
|---|---:|---:|---:|---:|---:|
| M | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| R | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| G | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| A | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| S | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| X | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |

### Notes

- quick_total_sum_raw: `<n>`
- result_status: `final`
```

## Quick Partial

```md
## Agent Spectrum Result

- version: `0.2.0`
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

### Evidence

| input | value | basis | note |
|---|---|---|---|
| active_model | `<value>` | `<observed|declared|inferred>` | `<note>` |
| tool_buckets | `<value>` | `<observed|declared|inferred>` | `<note>` |
| behavior_imprints | `<value>` | `<observed|declared|inferred>` | `<note>` |
| instinct_answers | `<missing|partial>` | `<declared>` | `<note>` |

### Totals

| axis | model | tools | instinct | quick_total_raw | quick_total_for_type |
|---|---:|---:|---:|---:|---:|
| M | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| R | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| G | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| A | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| S | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| X | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |

### Next Step

- result_status: `partial`
- next_action: `<ask-for-missing-inputs>`
```

## Deep Full

```md
## Agent Spectrum Result

- version: `0.2.0`
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

### Evidence

| input | value | basis | note |
|---|---|---|---|
| active_model | `<value>` | `<observed|declared|inferred>` | `<note>` |
| tool_buckets | `<value>` | `<observed|declared|inferred>` | `<note>` |
| behavior_imprints | `<value>` | `<observed|declared|inferred>` | `<note>` |
| instinct_answers | `<value>` | `<observed|declared|inferred>` | `<note>` |
| forced_ranking | `<value>` | `<declared>` | `<note>` |
| situational_answers | `<value>` | `<declared>` | `<note>` |
| behavior_traces | `<value>` | `<observed|declared|inferred>` | `<note>` |

### Totals

| axis | quick_total_raw | ranking | situations | behavior_traces | deep_total_raw | deep_total_for_type |
|---|---:|---:|---:|---:|---:|---:|
| M | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| R | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| G | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| A | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| S | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |
| X | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` | `<n>` |

### Guidance

- partner_direction: `<summary>`
- seven_day_plan: `<summary>`
- result_status: `final`
```

## Template Rules

- Use backticked scalar values for fixed metadata lines.
- Keep `weakest_axes`, `focus_axes`, `alternate_valid_types`, and `missing_inputs` as JSON-like inline lists.
- Keep axis rows in `M, R, G, A, S, X` order.
- Keep evidence rows in the order shown in the template.
- If a value is not available in a partial result, use `undetermined` rather than inventing a value.
