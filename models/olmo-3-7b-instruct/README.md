# Olmo 3 7B Instruct

**Our assessment:** With the patched template, record lookups and a short
publishing task worked. It still followed outdated instructions and made
malformed save calls.

**Official:** Ai2 7B text model, trained primarily for English. Apache 2.0
license. [Model details](https://huggingface.co/allenai/Olmo-3-7B-Instruct),
checked 13 September 2026.

Tested with **Q4_K_M**, thinking off and **16,384-token context**;
[tools-guard-v2 template](chat-templates.md).

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Observed result |
|---|---|
| Extraction | Correct fields and missing values |
| Classification | Correct queues, priorities and conversions |
| Reconciliation | Preserved time while changing rooms |
| Comprehension and synthesis | Reversed paid/unpaid status and a monitoring deadline |
| Reasoning | Correct constrained scheduling choice |
| Evidence assessment | Correctly checked all three claims |
| Drafting | Opening hours omitted; calendar condition invented |
| Editing | Changed a supplied quantity |
| Translation into Swedish | Garbled wording and reversed access directions |
| Following updated instructions | Returned the superseded appointment |
| Tool use | Found and read the current record |
| Planning and execution | Published and verified; plan added requirements |
| Recovery | Repeated malformed calls; no successful save |
| Saving and reporting results | Reported the failed save honestly |

## Performance

Measured 12 September 2026; the configurations above.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 238.2 | 24.4 | 12.23 GiB |

Rates are medians across 41 calls in 23 cases, with one server slot and prompt
caching enabled. Prefill counts newly processed input. Memory is sampled
resident memory (RSS) of the server process; it does not establish the
machine's RAM requirement. The tasks vary, which limits speed comparisons with
other workloads.

[All models](../README.md).
