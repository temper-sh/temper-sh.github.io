# Model card template

Copy the body into `models/<model>/README.md` for a general assistant, including
coding-focused assistants. For a pipeline component, use the separate
[specialist card](specialist-card.md). Replace the placeholders and include
only assessed tasks and measured configurations.

Lead with the work observed and its main limitation. Avoid broad suitability
labels, rankings or recommendations inferred from a few cases. The page must
stand on its own: summarize the test method, conditions, selected measurements
and failures. Labs and Workshop remain private; no log or research-repository
link is required. Link official sources and useful public comparisons.

<!-- {% raw %} -->

```markdown
# {{Model name}}

{{Observed work and the main limitation, in one or two sentences.}}

**Official:** {{Brief size, modalities, license and relevant context information.}}
[Official model details]({{official_source_url}}).

Tested with {{model build, engine version, relevant template, thinking setting,
context and tool assistance}}. {{State any official capability not assessed.}}

## Why this preset

{{Explain the concrete problem, requirement or comparison behind the selected
weights, engine and consequential settings, plus the cost or limit. Describe
relevant alternatives not selected for this setup and link fuller reasons in
the technical reference. Distinguish unevaluated, evaluated-but-unselected and
incompatible options. A mechanism description alone is not a reason. Identify
baseline/default settings rather than implying each value was tuned. Link the
installable preset. Use a small choice table for materially different presets.
Omit unsupported rationale and this section when there is no prepared preset.}}

## Observed tasks

{{Briefly describe the tasks, how answers were checked and the review limits.
Distinguish simulated tools, AI review and human acceptance where applicable.}}

| Task | Observed result |
|---|---|
| {{Work in ordinary language}} | {{Outcome and its important limit}} |

## Performance

{{State shared measurement conditions. Use one row per machine, with one
representative measured configuration and workload; put variants in details.}}

| Machine | Engine | Task and conditions | Time or rate | Loaded → peak memory |
|---|---|---|---:|---:|
| {{Chip/GPU, RAM, OS}} | {{Exact engine version}} | {{Work, input/output size and cache state}} | {{Measured result and unit}} | {{Measured amounts, units and metric; mark missing values}} |

{{Explain how rates and memory were summarized. Keep incorrect or unfinished
answers distinct from successful work; token speed is not time to accepted work.}}

{{State where this evidence does not transfer: other machines, settings,
longer work, or capabilities that remain unmeasured.}}

[Configuration and evidence]({{technical_reference_path}}).
```

<!-- {% endraw %} -->

Maintain one current assessment. Revise its prose and tables as evidence changes;
do not append dated updates, progress reports or next-work lists. Measurement and
source-check dates stay in retained provenance or optional evidence details.
Show dates in the main text only when timing changes a reader's decision or
action. Keep versions where they distinguish measurements or compatibility.

Compare configurations only when the difference helps a choice or explains a
limitation. Keep the main hardware table to one row per machine; put additional
workloads and configurations in optional evidence details. Do not combine timing,
quality and memory from different runs into one apparent result. A current preset
using newer software or settings does not inherit an older measurement.

A single configuration can stay inline. Use `layouts/<configuration>.md` for a
distinct preset choice and `layouts/<configuration>-reference.md` for detail
that would crowd its short page. The reference retains exact configurations,
methods, provenance and useful failures; decision-changing limits stay on the
short page. Keep shared official facts on the main card. Use `chat-templates.md`
and `tuning.md` for guidance when there are actual findings to explain.

References distinguish published presets from evaluated variants. Current
installation settings come from the signed catalog, while old measurements
retain their own configuration. Indexes link to the facts instead of copying
their measurements. Omit the reference link when no separate page is needed.

Use the memory metric actually measured: GPU allocation and process resident
memory are different. A peak on one machine does not establish minimum RAM.
The [model index](../models/README.md) explains memory tiers and estimated fit.
