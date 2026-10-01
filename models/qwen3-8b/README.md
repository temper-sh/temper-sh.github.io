# Qwen3 8B

**Our assessment:** Completed short English drafts, extraction and
copyediting. Some workflows omitted final checks; Swedish translation needed
repairs.

**Official:** Qwen 8.2B text model with optional thinking. Apache 2.0;
32,768-token native context, extendable with YaRN. [Model
details](https://huggingface.co/Qwen/Qwen3-8B), checked 13 September 2026.

Tested with **Q4_K_M**, thinking off and **16,384-token context**.

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Observed result |
|---|---|
| Extraction | Correct fields and missing values |
| Classification | Correct queues, priorities and conversions |
| Reconciliation | Preserved time while changing rooms |
| Comprehension and synthesis | Correct policy decisions with contradictory wording; summary omissions |
| Reasoning | Correct constrained scheduling choice |
| Evidence assessment | Correctly checked all three claims |
| Drafting | Both short English drafts complete |
| Editing | Completed the constrained copyedit |
| Translation into Swedish | Short text needed substantial repair; longer text needed specific corrections |
| Following updated instructions | Returned the latest arrangement |
| Tool use | Found and read the current record |
| Planning and execution | Valid plan; published without the required final read |
| Recovery | Preserved the concurrent edit but omitted reading the saved result |
| Saving and reporting results | Saved, reread and validated the change |

## Performance

Measured 12 September 2026; the configurations above.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 310.9 | 23.2 | 6.69 GiB |

Rates are medians across 38 calls in 23 cases, with one server slot and prompt
caching enabled. Prefill counts newly processed input. Memory is sampled
resident memory (RSS) of the server process; it does not establish the
machine's RAM requirement. The tasks vary, which limits speed comparisons with
other workloads.

[All models](../README.md).
