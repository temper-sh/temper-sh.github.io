# Specialist card template

Use for a component with a defined pipeline job, such as extraction, reranking
or translation. Coding and writing assistants use the [general model card](model-card.md).
Keep one family card when variants share a job and their differences help a
choice. Separate regional or size variants in the comparison where needed.

Start with the input/output contract and the documented configuration rationale.
Include language coverage, a clickable license, engine/device choices and
resource costs. Distinguish publisher guidance, local integration checks and
task-quality evidence. Describe publisher coverage separately from local checks;
do not present an unmeasured language or variant as locally validated. Minimize
judgment and recommendations; a few cases do not establish broad reliability.

<!-- {% raw %} -->

```markdown
# {{Specialist or family}}

{{The component's input/output job, observed behavior and main limit.}}

**Official:** {{Size, modalities, language coverage and relevant limits.}}
[{{License and material restriction}}]({{license_explanation_url}}).
[Publisher details]({{source_url}}).

## Choose a variant

{{Include only when variants offer a meaningful choice: languages, size,
engine or device. Explain the differences and the source of each claim.}}

## Why this preset

{{Explain the requirement or observed problem behind the weights, engine/device,
settings and adapters, with their costs and limits. Retain reasons for relevant
alternatives not selected here; distinguish them from unevaluated or incompatible
options. A component's function alone does not explain its selection. Identify
inherited settings instead of implying independent tuning. Link fuller decision
notes and the offered preset. Do not assume every alternative was compared.}}

## Use in a pipeline

**{{Input → output.}}** {{What the caller supplies, the request interface,
what the next step consumes, and what needs checking. Link the product's
request guide for exact API syntax.}}

## {{Record correctness / Ranking quality / Translation quality}}

{{Tested configuration and question, method, source or oracle, and review limits.}}

| Task or comparison | Observed result |
|---|---|
| {{Question within this specialist's job}} | {{Outcome, including errors or absent evidence}} |

## Performance

{{State shared engine/settings and measurement conditions. Use one row per
machine; put additional variants and workloads in evidence details.}}

| Machine | Configuration | Workload | Request time | Loaded → peak memory |
|---|---|---|---:|---:|
| {{Chip/GPU, RAM, OS}} | {{Weights, engine, device}} | {{Input/output size or candidate count}} | {{Time, unit, summary}} | {{Amounts, units, metric; mark missing values}} |

{{State startup/cache conditions and whether timings include wrong outputs.
Separate process memory from minimum RAM; name unmeasured co-residency and
pipeline overhead. Do not imply time saved in completed work from component
latency alone.}}

[Configuration and evidence]({{technical_reference_path}}).
```

<!-- {% endraw %} -->

Use task-specific evidence: record selection, associations and absence for
extraction; candidate retrieval and ordering for reranking; preserved meaning
by language pair for translation. Prefill or token rates belong only where they
help explain a choice. Do not score unrelated general-assistant abilities or
combine these jobs into one specialist score. Omit empty sections and invented
measurements. Maintain the current assessment in place.

Move detailed comparisons, conditions and evidence into a linked public
technical reference when they crowd the short page. Keep important limitations
visible. Use the general card's reference convention; no reference page or
link is needed when the material already fits. Installation values remain
catalog-owned, while measurements retain their exact tested configurations.

The main hardware table has one row per machine. Choose a representative
measured configuration and workload, keeping meaningful variant comparisons
in optional details. Never combine figures from different runs into one result.

Keep dates and progress narration out of the main reading path. Retain
measurement and source-check dates in provenance or optional evidence details;
show a date in the main text only when timing changes a reader's decision or
action. Versions remain visible where they distinguish configurations or
compatibility.
