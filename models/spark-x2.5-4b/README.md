# Spark X2.5 4B

**Our assessment:** With thinking on, completed short policy, fact-preserving
copyediting and recovery tasks on the official engine. Longer writing remained
incomplete in broader tests on the XHToken fork; both thinking settings
changed meaning when translating into Swedish.

**Official:** XHToken 4B text model with hybrid attention and optional
thinking. Apache 2.0; 1,048,576-token native context. [Model
details](https://huggingface.co/XHToken/Spark-X2.5-4B), checked 13 September
2026.

Tested with **Q4_K_M**, the embedded template and **16,384-token context**.
Answer allowances were 4,096 tokens with thinking off and 8,192 with thinking
on. The official b11149 configuration used llama-swap v257; the fork was
XHToken `a698f1c`. Engine and launch settings differ between configurations.

## Capabilities

AI reviewers checked answers against supplied sources, using simulated tools.
No person independently accepted the work. The official-engine column covers
five focused cases; each fork column covers 23 cases. An untested capability
remains unknown for that configuration.

| Capability | Official b11149 · thinking on | XHToken fork · thinking on | XHToken fork · thinking off |
|---|---|---|---|
| Extraction | Not assessed | Correct fields | Correct fields |
| Classification | Not assessed | Correct queues, priorities and conversions | Correct queues, priorities and conversions |
| Reconciliation | Not assessed | Correct room update | Correct room update |
| Comprehension and synthesis | Preserved approval, temporary cap and sponsor conditions | Policy complete; another summary omitted status | Invented timing and policy permission |
| Reasoning | Not assessed | Correct scheduling choice | Correct scheduling choice |
| Evidence assessment | Three claims correct, including absent evidence | Three claims correct | Three claims correct |
| Drafting | Not assessed | Short notice complete; calendar wording repair | Allowed a closed time; omitted attribution |
| Editing | Corrected prose while preserving plural forms, Friday and address | Completed the constrained copyedit | Changed a supplied quantity |
| Translation into Swedish | Not assessed | Meaning changes present | Directions and requirements changed |
| Following updated instructions | Not assessed | Latest arrangement correct | Latest arrangement correct |
| Tool use | Found and read the current record | Found and read the record | Found record, omitted the requested read |
| Planning and execution | Not assessed | Planned, published and verified | Planned, published and verified |
| Recovery | Preserved concurrent edit, saved and read back the change | Preserved and verified edit | Preserved edit; omitted reading the saved result |
| Saving and reporting results | Accurately reported the recovered edit; separate completion cases unassessed | Saved, reread and validated | Saved, reread and validated |

## Performance

| Configuration / workload | Measured | Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---|---|---:|---:|---:|
| Thinking on · five focused cases | 24 Sep 2026 | Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b11149 | — | — | 2.94 GiB |
| Thinking on · 23 suite cases | 12 Sep 2026 | Apple M5 · 32 GiB · macOS 26.6.1 | XHToken llama.cpp 0.1.2-dev-build-1 · a698f1c | 474.6 | 37.3 | 4.52 GiB |
| Thinking off · 23 suite cases | 12 Sep 2026 | Apple M5 · 32 GiB · macOS 26.6.1 | XHToken llama.cpp 0.1.2-dev-build-1 · a698f1c | 526.9 | 39.2 | 4.03 GiB |

Fork rates are medians across 39 / 38 calls respectively; no rate summary is
shown for the focused workload. One server slot, prompt caching enabled.
Prefill counts newly processed input; generation includes reasoning tokens.
Memory is sampled server RSS, not minimum machine RAM. Workloads differ, so
these rows do not isolate an engine improvement. Context capacity and fit on
8/16 GiB machines remain unmeasured.

[All models](../README.md).
