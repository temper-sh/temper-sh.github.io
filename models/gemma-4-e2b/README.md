# Gemma 4 E2B

**Our assessment:** Extracted fields and completed short tool tasks. Some
writing changed source facts, and recovery from a conflicting edit stopped
before saving.

**Official:** Google DeepMind model with 2.3B effective parameters (5.1B
including embeddings), text/image/audio input and text output. Apache 2.0;
128k context. [Model details](https://huggingface.co/google/gemma-4-E2B-it),
checked 13 September 2026.

Tested with **Q4_K_M**, the embedded template, thinking off and **16,384-token
context**. The b11149 configuration used llama-swap v257 and a 4,096-token
output allowance. Text only; image and audio unassessed.

## Capabilities

AI reviewers checked answers against supplied sources, using simulated tools.
No person independently accepted the work. The b11149 column covers five
focused cases; b10655 covers the broader 23-case suite. Results remain
attached to the configuration tested.

| Capability | llama.cpp b11149 | llama.cpp b10655 |
|---|---|---|
| Extraction | Correct fields and missing values | Correct fields and missing values |
| Classification | Not assessed | Correct queues, priorities and conversions |
| Reconciliation | Not assessed | Preserved time while changing rooms |
| Comprehension and synthesis | Not assessed | Misapplied a policy restriction; omitted current status |
| Reasoning | Not assessed | Correct constrained scheduling choice |
| Evidence assessment | Treated absent signature evidence as contradiction | Two claims correct; treated absence as contradiction |
| Drafting | Not assessed | Calendar message complete; access notice allowed a closed time |
| Editing | Changed plural “forms” to singular | Changed a supplied quantity and lost punctuation |
| Translation into Swedish | Not assessed | Specific wording and qualification corrections needed |
| Following updated instructions | Returned the latest arrangement | Returned the latest arrangement |
| Tool use | Not assessed separately | Found and read the current record |
| Planning and execution | Not assessed | Planned, published and verified the short release |
| Recovery | Reread concurrent change, then stopped without saving | Read the concurrent change, then stopped without saving |
| Saving and reporting results | Accurately reported unfinished recovery; separate completion cases unassessed | Saved and verified; a separate validation-only case omitted the required read |

## Performance

Broader suite, measured 12 September 2026.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 756.0 | 57.6 | 3.26 GiB |

Rates are medians across 38 calls in 23 cases, with one server slot and prompt
caching enabled. Prefill counts newly processed input. Memory is sampled
resident memory (RSS) of the server process; it does not establish the
machine's RAM requirement. The tasks vary, which limits speed comparisons with
other workloads.

On b11149/v257, the model retrieved facts spread through 11,776 initial input
tokens and answered a follow-up within the 16,384-token window, retaining a
4,096-token output allowance. The values were correct, but Markdown fences
around them failed the separate check for plain JSON.

Measured 24 September 2026 on the same M5/32 GiB, lookup took 12.09 seconds
and continuation 1.16 seconds, with 3.18 GiB peak sampled server RSS and no
observed swap growth. This is a tested context point, not a maximum or
evidence of fit on 8/16 GiB machines.

[All models](../README.md).
