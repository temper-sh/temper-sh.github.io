# Granite 4.2 3B

**Our assessment:** Both reasoning settings completed extraction and
copyediting. Low reasoning preserved another edit to the same file; reasoning
off overwrote it. Other answers still changed source facts.

**Official:** IBM 3B dense text model with reasoning support. Apache 2.0; 128k
native context. [Model
details](https://huggingface.co/ibm-granite/granite-4.2-3b), checked 13
September 2026.

Tested with **Q4_K_M**, **16,384-token context**, and the two settings below.
Answer allowances were 4,096 tokens off and 8,192 with reasoning. The
comparison therefore includes changes to both reasoning and the output
allowance.

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Reasoning off | Low reasoning |
|---|---|---|
| Extraction | Correct fields | Correct fields |
| Classification | Correct queues, priorities and conversions | Correct queues, priorities and conversions |
| Reconciliation | Correct room update | Correct room update |
| Comprehension and synthesis | Contradictory dates and speculation | Invented approval/payment condition |
| Reasoning | Correct scheduling choice | Correct choice; contradictory explanation |
| Evidence assessment | Labels correct; overclaimed missing evidence | False signature-evidence conclusion |
| Drafting | Short notice complete; calendar omissions | Short notice complete; calendar claims needed repair |
| Editing | Completed the constrained copyedit | Completed the constrained copyedit |
| Translation into Swedish | Substantial meaning changes | English left untranslated; entrance status reversed |
| Following updated instructions | Latest arrangement correct | Latest arrangement correct |
| Tool use | Found and read the record | Found and read the record |
| Planning and execution | Valid plan; final publish read omitted | Plan invented passed checks; final read omitted |
| Recovery | Overwrote concurrent P9 with outdated P8 | Preserved and verified P9 |
| Saving and reporting results | Saved, reread and validated | Saved, reread and validated |

## Performance

Measured 12 September 2026; the configurations above.

| Configuration | Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---|---:|---:|---:|
| Off | Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 594.0 | 48.2 | 3.56 GiB |
| Low | Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 631.4 | 47.2 | 3.46 GiB |

Rates are medians across 40 / 39 calls in 23 cases per setting, with one
server slot and prompt caching enabled. Prefill counts newly processed input.
Generation includes reasoning tokens when enabled. Memory is sampled resident
memory (RSS) of the server process; it does not establish the machine's RAM
requirement. The tasks vary, which limits speed comparisons with other
workloads.

[All models](../README.md).
