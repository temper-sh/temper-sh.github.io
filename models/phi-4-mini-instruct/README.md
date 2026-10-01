# Phi 4 Mini Instruct

**Our assessment:** Completed field extraction and a short copyedit. It
invented tool results and claimed saved work without making the calls.

**Official:** Microsoft 3.8B text model. MIT license; 128k context. [Model
details](https://huggingface.co/microsoft/Phi-4-mini-instruct), checked 13
September 2026.

Tested with **Q4_K_M**, thinking off and **16,384-token context**.

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Observed result |
|---|---|
| Extraction | Correct fields and missing values |
| Classification | Misrouted a login issue; lost the missing-value rule |
| Reconciliation | Preserved time while changing rooms |
| Comprehension and synthesis | Approved a forbidden booking and omitted a handover time |
| Reasoning | Correct choice with a false constraint explanation |
| Evidence assessment | Reversed the order of two times |
| Drafting | Access and calendar messages contained false facts |
| Editing | Completed the constrained copyedit |
| Translation into Swedish | Substantial meaning changes |
| Following updated instructions | Returned the latest arrangement |
| Tool use | No actual calls; invented record contents |
| Planning and execution | Plan needed repair; release execution only promised |
| Recovery | Invented recovery without performing it |
| Saving and reporting results | Claimed an unperformed save |

## Performance

Measured 12 September 2026; the configurations above.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 550.8 | 41.8 | 3.96 GiB |

Rates are medians across 23 calls in 23 cases, with one server slot and prompt
caching enabled. Prefill counts newly processed input. Memory is sampled
resident memory (RSS) of the server process; it does not establish the
machine's RAM requirement. The tasks vary, which limits speed comparisons with
other workloads.

[All models](../README.md).
