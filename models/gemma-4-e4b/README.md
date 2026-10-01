# Gemma 4 E4B

**Our assessment:** Useful for short drafts from supplied facts, extraction
and simple tool workflows. Longer editing retained false claims after
feedback.

**Official:** Google DeepMind model with 4.5B effective parameters (8B
including embeddings), text/image/audio input and text output. Apache 2.0;
128k context. [Model details](https://huggingface.co/google/gemma-4-E4B-it),
checked 13 September 2026.

Tested with **Q4_K_M**, the embedded template, thinking off and **16,384-token
context**. The b11149 configuration used llama-swap v257 and a 4,096-token
output allowance. Text only; image and audio unassessed.

## Capabilities

AI reviewers checked answers against supplied sources, using simulated tools.
No person independently accepted the work. The b11149 column covers five
focused cases; b10621 covers the broader 23-case suite. Results remain
attached to the configuration tested.

| Capability | llama.cpp b11149 | llama.cpp b10621 |
|---|---|---|
| Extraction | Correct fields and missing values | Correct fields and missing values |
| Classification | Not assessed | Correct queues; missing priority became “Unknown” instead of REVIEW |
| Reconciliation | Not assessed | Preserved time while changing rooms |
| Comprehension and synthesis | Not assessed | Amended policy understood; another summary omitted current status |
| Reasoning | Not assessed | Correct constrained scheduling choice |
| Evidence assessment | Mislabeled absent signature evidence and on-time delivery | Confused missing evidence with contradiction |
| Drafting | Not assessed | Both short English drafts complete |
| Editing | Changed plural “forms” to singular | Fixed grammar but changed a supplied quantity |
| Translation into Swedish | Not assessed | Wording, grammar and qualification repairs needed |
| Following updated instructions | Returned the latest arrangement | Returned the latest arrangement |
| Tool use | Not assessed separately | Found and read the current record |
| Planning and execution | Not assessed | Planned, published and verified the short release |
| Recovery | Preserved concurrent edit, saved and read back the change | Preserved and verified a concurrent edit |
| Saving and reporting results | Accurately reported the recovered edit; separate completion cases unassessed | Saved, reread and validated the change |

## Performance

Broader suite, measured 12 September 2026.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10621 | 495.9 | 35.7 | 5.40 GiB |

Rates are medians across 41 calls in 23 cases, with one server slot and prompt
caching enabled. Prefill counts newly processed input. Memory is sampled
resident memory (RSS) of the server process; it does not establish the
machine's RAM requirement. The tasks vary, which limits speed comparisons with
other workloads.

On b11149/v257, the model retrieved facts spread through 11,776 initial input
tokens and answered a follow-up within the 16,384-token window, retaining a
4,096-token output allowance. The values were correct, but Markdown fences
around them failed the separate check for plain JSON.

Measured 24 September 2026 on the same M5/32 GiB, lookup took 18.88 seconds
and continuation 2.00 seconds, with 5.02 GiB peak sampled server RSS and no
observed swap growth. This is a tested context point, not a maximum or
evidence of fit on 8/16 GiB machines.

[All models](../README.md).
