# Model card template

Copy the body into `models/<model>/README.md` for a general assistant, including
coding-focused assistants. For a pipeline component, use the separate
[specialist card](specialist-card.md). Replace the placeholders and include
only assessed tasks and measured configurations.

Lead with what the model is useful for and its main limitation. The page must
stand on its own: summarize the test method, conditions, selected measurements
and failures. Labs and Workshop remain private; no log or research-repository
link is required. Link official sources and useful public comparisons.

<!-- {% raw %} -->

```markdown
# {{Model name}}

**Our assessment:** {{Useful work and the main limitation, in one or two sentences.}}

**Official:** {{Brief size, modalities, license and relevant context information.}}
[Official model details]({{official_source_url}}), checked {{source_check_date}}.

Tested with {{model build, engine version, relevant template, thinking setting,
context and tool assistance}}. {{State any official capability not assessed.}}

## Capabilities

{{Briefly describe the tasks, how answers were checked and the review limits.
Distinguish simulated tools, AI review and human acceptance where applicable.}}

| Task | Observed result |
|---|---|
| {{Work in ordinary language}} | {{Outcome and its important limit}} |

## Performance

Measured {{date or dates}}, using the configurations shown.

| Machine | Engine | Task and conditions | Time or rate | Peak memory |
|---|---|---|---:|---:|
| {{Chip/GPU, RAM, OS}} | {{Exact engine version}} | {{Work, input/output size and cache state}} | {{Measured result and unit}} | {{Measured amount and metric}} |

{{Explain how rates and memory were summarized. Keep incorrect or unfinished
answers distinct from successful work; token speed is not time to accepted work.}}

{{State where this evidence does not transfer: other machines, settings,
longer work, or capabilities that remain unmeasured.}}
```

<!-- {% endraw %} -->

Maintain one current assessment. Revise its prose and tables as evidence changes;
do not append dated updates, progress reports or next-work lists. Dates and
versions identify measurements, not the structure of the page.

Compare configurations only when the difference helps a choice or explains a
limitation. Keep each result attached to its tested setup. A current preset
using newer software or settings does not inherit an older measurement.

A single configuration can stay inline. Use `layouts/<configuration>.md` when
a useful comparison needs more space, or `chat-templates.md` and `tuning.md` for
optional technical guidance. Keep shared official facts on the main card.

Use the memory metric actually measured: GPU allocation and process resident
memory are different. A peak on one machine does not establish minimum RAM.
The [model index](../models/README.md) explains memory tiers and estimated fit.
