# MiniCPM5 1B

**Our assessment:** Completed simple field extraction and one lookup. Other
tasks produced errors or false reports of saved work.

**Official:** OpenBMB text model with about 1.08B parameters and optional
thinking. Apache 2.0; 131,072-token context. [Model
details](https://huggingface.co/openbmb/MiniCPM5-1B), checked 13 September
2026.

Tested with **Q4_K_M**, thinking off and **16,384-token context**.

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Observed result |
|---|---|
| Extraction | Correct fields and missing values |
| Classification | Reversed queues; boundary and conversion errors |
| Reconciliation | Refused the requested room update |
| Comprehension and synthesis | Invented conditions and approved a forbidden booking |
| Reasoning | Correct choice with a contradictory explanation |
| Evidence assessment | Misread delivery times and arrival evidence |
| Drafting | Both English messages required corrections |
| Editing | Changed a protected email address |
| Translation into Swedish | Fragmented output did not deliver the translation |
| Following updated instructions | Correct day and time; omitted the room |
| Tool use | Search/read worked; direct read was refused |
| Planning and execution | Published without final read; plan invented completed checks |
| Recovery | Reported no tools and did not recover |
| Saving and reporting results | Claimed success after a failed save |

## Performance

Measured 12 September 2026; the configurations above.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10621 | 3,327.3 | 174.6 | 1.14 GiB |

Rates are medians across 31 calls in 23 cases, with one server slot and prompt
caching enabled. Prefill counts newly processed input. Memory is sampled
resident memory (RSS) of the server process; it does not establish the
machine's RAM requirement. The tasks vary, which limits speed comparisons with
other workloads.

[All models](../README.md).
