## Agent Spectrum Result

- version: `0.2.0`
- mode: `deep`
- is_partial: `false`
- primary_axis: `A`
- secondary_axis: `R`
- type_pair: `A+R`
- type: `Calm Executor`
- faction: `⚖️ Balancers`
- weakest_axes: [`S`, `X`]
- focus_axes: [`S`, `X`]
- tie_break: `none`
- alternate_valid_types: []
- overrides_quick_result: `false`

### Evidence

| input | value | basis | note |
|---|---|---|---|
| active_model | `GPT-5` | `observed` | normalized to the `GPT-4o / o3 / o4` bucket |
| tool_buckets | `code executor, search tool, browser automation, data analysis tool` | `observed` | all four are callable in-session |
| behavior_imprints | `published public content` | `declared` | counted only in quick scoring |
| instinct_answers | `Q1=A, Q2=A, Q3=B` | `declared` | quick edition completed before deep |
| forced_ranking | `R=1, A=2, M=3, G=4, X=5, S=6` | `declared` | no duplicated ranks |
| situational_answers | `Q4=A, Q5=A, Q6=A, Q7=A, Q8=A, Q9=A, Q10=B, Q11=B, Q12=B` | `declared` | deep situational set completed |
| behavior_traces | `completed zero-to-one project` | `declared` | counted only in deep behavior traces |

### Totals

| axis | quick_total_raw | ranking | situations | behavior_traces | deep_total_raw | deep_total_for_type |
|---|---:|---:|---:|---:|---:|---:|
| M | 10 | 3 | 15 | 0 | 28 | 28 |
| R | 53 | 15 | 15 | 0 | 83 | 83 |
| G | 10 | 0 | 0 | 8 | 18 | 18 |
| A | 45 | 8 | 15 | 8 | 76 | 76 |
| S | 0 | 0 | 0 | 0 | 0 | 0 |
| X | 0 | 0 | 0 | 0 | 0 | 0 |

### Guidance

- partner_direction: `Seek S-strong types first, then X-strong types, to compensate for the tied weakest axes.`
- seven_day_plan: `Prioritize S first and X second over the next 7 days.`
- result_status: `final`
