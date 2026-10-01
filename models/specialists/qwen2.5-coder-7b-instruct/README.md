# Qwen2.5 Coder 7B Instruct

**Our assessment:** Completed plain-text extraction, reasoning and short
drafting. It described tool actions without executing them, leaving repository
tasks unfinished. These tests do not establish broader coding ability.

**Official:** Qwen coding-focused text model with 7.61B parameters. Apache
2.0; 32,768-token configured context, extendable with YaRN. [Model
details](https://huggingface.co/Qwen/Qwen2.5-Coder-7B-Instruct), checked 13
September 2026.

Tested with **Q4_K_M**, thinking off and **16,384-token context**.

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Observed result |
|---|---|
| Extraction | Correct fields and missing values |
| Classification | Correct queues; lost the missing-value priority rule |
| Reconciliation | Preserved time while changing rooms |
| Comprehension and synthesis | Contradictory limits and omitted status |
| Reasoning | Correct constrained scheduling choice |
| Evidence assessment | Correctly checked all three claims |
| Drafting | Short access message complete; calendar wording needed repair |
| Editing | Changed a supplied quantity |
| Translation into Swedish | Changed a protected address and photo-ID requirement |
| Following updated instructions | Returned the latest arrangement |
| Tool use | Printed tool-shaped text without actual calls |
| Planning and execution | Valid short plan; release not executed |
| Recovery | No actual recovery |
| Saving and reporting results | No actual saved change |

## Performance

Measured 12 September 2026; the configurations above.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 333.2 | 24.4 | 5.17 GiB |

Rates are medians across 23 calls in 23 cases, with one server slot and prompt
caching enabled. Prefill counts newly processed input. Memory is sampled
resident memory (RSS) of the server process; it does not establish the
machine's RAM requirement. The tasks vary, which limits speed comparisons with
other workloads.

[All models](../../README.md).
