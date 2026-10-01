# Ornith 1.5 9B

**Our assessment:** Completed short evidence checks and tool workflows. Policy
interpretation and longer prose needed correction.

**Official:** Ornith AI 9B dense text-and-vision model based on Qwen3.5. MIT
license. [Model details](https://huggingface.co/ornith-ai/Ornith-1.5-9B),
checked 13 September 2026.

Tested with **Q4_K_M**, thinking off and **16,384-token context**. Text only;
vision unassessed.

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Observed result |
|---|---|
| Extraction | Correct fields and missing values |
| Classification | Correct queues, priorities and conversions |
| Reconciliation | Preserved time while changing rooms |
| Comprehension and synthesis | Reversed paid/unpaid status and changed an investigation owner |
| Reasoning | Correct constrained scheduling choice |
| Evidence assessment | Correctly checked all three claims |
| Drafting | Short access message complete; calendar attribution missing |
| Editing | Changed a supplied quantity |
| Translation into Swedish | Short wording repairs; substantial changes in longer text |
| Following updated instructions | Returned the latest arrangement |
| Tool use | Found and read the current record |
| Planning and execution | Planned, published and verified the short release |
| Recovery | Preserved and verified a concurrent edit |
| Saving and reporting results | Saved, reread and validated the change |

## Performance

Measured 12 September 2026; the configurations above.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 246.2 | 19.9 | 8.84 GiB |

Rates are medians across 40 calls in 23 cases, with one server slot and prompt
caching enabled. Prefill counts newly processed input. Memory is sampled
resident memory (RSS) of the server process; it does not establish the
machine's RAM requirement. The tasks vary, which limits speed comparisons with
other workloads.

[All models](../README.md).
