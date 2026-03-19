## Agent Spectrum Result

- version: `0.2.0`
- mode: `quick`
- is_partial: `true`
- primary_axis: `R`
- secondary_axis: `A`
- type_pair: `A+R`
- type: `Calm Executor`
- faction: `⚖️ Balancers`
- weakest_axes: [`G`, `S`, `X`]
- focus_axes: [`G`, `S`]
- tie_break: `incomplete`
- alternate_valid_types: []
- missing_inputs: [`Q1`, `Q2`, `Q3`]

### Evidence

| input | value | basis | note |
|---|---|---|---|
| active_model | `GPT-5` | `observed` | normalized to the `GPT-4o / o3 / o4` bucket |
| tool_buckets | `code executor, search tool` | `observed` | browser automation was not available |
| behavior_imprints | `none` | `declared` | no public content claimed |
| instinct_answers | `missing` | `declared` | user did not answer the quick instinct questions |

### Totals

| axis | model | tools | instinct | quick_total_raw | quick_total_for_type |
|---|---:|---:|---:|---:|---:|
| M | 0 | 0 | 0 | 0 | 0 |
| R | 15 | 18 | 0 | 33 | 33 |
| G | 0 | 0 | 0 | 0 | 0 |
| A | 15 | 8 | 0 | 23 | 23 |
| S | 0 | 0 | 0 | 0 | 0 |
| X | 0 | 0 | 0 | 0 | 0 |

### Next Step

- result_status: `partial`
- next_action: `ask-for-missing-inputs`
