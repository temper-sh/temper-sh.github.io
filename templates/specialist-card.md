# Specialist card template

Use for a component with a defined pipeline job, such as extraction, reranking
or translation. Coding and writing assistants use the [general model card](model-card.md).
Keep one family card when variants share a job and their differences help a
choice. Separate regional or size variants in the comparison where needed.

Start with the input/output contract and the reason to choose this component.
Include language coverage, a clickable license, engine/device choices and
resource costs. Distinguish publisher guidance, local integration checks and
task-quality evidence. A recommendation may rely on trusted publisher evidence;
do not present an unmeasured language or variant as locally validated.

```markdown
# {{Specialist or family}}

**Our assessment:** {{Which pipeline step it suits, why choose it, and its main limit.}}

**Official:** {{Size, modalities, language coverage and relevant limits.}}
[{{License and material restriction}}]({{license_explanation_url}}).
[Publisher details]({{source_url}}), checked {{date}}.

## Choose a variant

{{Include only when variants offer a meaningful choice: languages, size,
engine or device. State the evidence supporting each recommendation.}}

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

Measured {{date}}, on {{machine, RAM, OS}}, with {{exact engine and settings}}.

| Configuration | Workload | Request time | Peak memory |
|---|---|---:|---:|
| {{Weights, engine, device}} | {{Input/output size or candidate count}} | {{Time, unit, summary}} | {{Amount, unit, metric}} |

{{State startup/cache conditions and whether timings include wrong outputs.
Separate process memory from minimum RAM; name unmeasured co-residency and
pipeline overhead. Do not imply time saved in completed work from component
latency alone.}}
```

Use task-specific evidence: record selection, associations and absence for
extraction; candidate retrieval and ordering for reranking; preserved meaning
by language pair for translation. Prefill or token rates belong only where they
help explain a choice. Do not score unrelated general-assistant abilities or
combine these jobs into one specialist score. Omit empty sections and invented
measurements. Maintain the current assessment in place.
