# Granite 4.2 8B

**Our assessment:** Completed extraction, copyediting and short tool tasks.
Policy summaries and calendar messages still changed the meaning of the
source.

**Official:** IBM 8B dense text model with reasoning support. Apache 2.0; 128k
native context. [Model
details](https://huggingface.co/ibm-granite/granite-4.2-8b), checked 13
September 2026.

Tested with **Q4_K_M**, thinking off and **16,384-token context**.

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Observed result |
|---|---|
| Extraction | Correct fields and missing values |
| Classification | Correct queues, priorities and conversions |
| Reconciliation | Preserved time while changing rooms |
| Comprehension and synthesis | Omitted status and broadened a policy prohibition |
| Reasoning | Correct constrained scheduling choice |
| Evidence assessment | Treated absent evidence as contradiction |
| Drafting | Short access message complete; calendar message invented facts |
| Editing | Completed the constrained copyedit |
| Translation into Swedish | Substantial repairs, including changed access instructions |
| Following updated instructions | Returned the latest arrangement |
| Tool use | Found and read the current record |
| Planning and execution | Valid plan; published without the required final read |
| Recovery | Preserved and verified a concurrent edit |
| Saving and reporting results | Saved, reread and validated the change |

## Performance

Measured 12 September 2026; the configurations above.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 261.7 | 20.8 | 7.70 GiB |

Rates are medians across 40 calls in 23 cases, with one server slot and prompt
caching enabled. Prefill counts newly processed input. Memory is sampled
resident memory (RSS) of the server process; it does not establish the
machine's RAM requirement. The tasks vary, which limits speed comparisons with
other workloads.

[All models](../README.md).
