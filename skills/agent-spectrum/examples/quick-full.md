## Agent Spectrum Result

- version: `0.2.0`
- mode: `quick`
- is_partial: `false`
- primary_axis: `R`
- secondary_axis: `A`
- type_pair: `A+R`
- type: `Calm Executor`
- faction: `⚖️ Balancers`
- weakest_axes: [`G`, `S`, `X`]
- focus_axes: [`G`, `S`]
- tie_break: `none`
- alternate_valid_types: []

### Evidence

| input | value | basis | note |
|---|---|---|---|
| active_model | `GPT-5` | `observed` | normalized to the `GPT-4o / o3 / o4` bucket |
| tool_buckets | `code executor, search tool` | `observed` | both are callable in-session |
| behavior_imprints | `none` | `declared` | no public content claimed |
| instinct_answers | `Q1=A, Q2=A, Q3=B` | `declared` | all quick questions answered |

### Totals

| axis | model | tools | instinct | quick_total_raw | quick_total_for_type |
|---|---:|---:|---:|---:|---:|
| M | 0 | 0 | 10 | 10 | 10 |
| R | 15 | 18 | 10 | 43 | 43 |
| G | 0 | 0 | 0 | 0 | 0 |
| A | 15 | 8 | 10 | 33 | 33 |
| S | 0 | 0 | 0 | 0 | 0 |
| X | 0 | 0 | 0 | 0 | 0 |

### Notes

- quick_total_sum_raw: `86`
- result_status: `final`
