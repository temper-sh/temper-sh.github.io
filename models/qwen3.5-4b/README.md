# Qwen3.5 4B

**Our assessment:** Useful for extracting fields, classifying records and
short tool workflows. Editing, longer writing and translation need close
checking: fluent answers can change facts or retain errors after feedback.

**Official:** Qwen's 4-billion-parameter text-and-vision model. Apache 2.0
license; 262,144-token native context. [Model
details](https://huggingface.co/Qwen/Qwen3.5-4B), checked 12 September 2026.

Tested with the compressed **Q4_K_M** build, embedded template, thinking off
and a **16,384-token context**. The b11149 configuration used llama-swap v257
and a 4,096-token output allowance. Those capability tests used text input.
The separate structured-extraction comparison below includes one invoice image
and uses its own configuration.

## Capabilities

AI reviewers checked answers against supplied sources, using simulated tools.
The b11149 column covers five focused cases; b10621 covers the broader 23-case
suite. Each configuration keeps its own results; differences cannot be
attributed solely to the engine version.

| Capability | llama.cpp b11149 | llama.cpp b10621 |
|---|---|---|
| Extraction | Correct fields and missing values | Correct fields and missing values |
| Classification | Not assessed | Correct categories, priority rules and unit conversions |
| Reconciliation | Not assessed | Applied a room change while preserving the time |
| Comprehension and synthesis | Not assessed | Useful summaries with omissions and contradictions |
| Reasoning | Not assessed | Solved a small scheduling problem with two constraints |
| Evidence assessment | Correctly checked three claims | Correctly checked three claims |
| Drafting | Not assessed | Short notice complete; longer messages contained false claims |
| Editing | Changed plural “forms” to singular | Fixed grammar but changed a supplied quantity |
| Translation into Swedish | Not assessed | Grammar errors and changes to instructions |
| Following updated instructions | Returned the latest arrangement | Returned a superseded appointment time |
| Tool use | Not assessed separately | Found and read the correct current record |
| Planning and execution | Not assessed | Completed a short publish workflow; planning added requirements |
| Recovery | Preserved concurrent edit, saved and read back the change | Preserved a concurrent edit when retrying a save |
| Saving and reporting results | Accurately reported the recovered edit; separate completion cases unassessed | Saved, reread and verified changes correctly |

An actual investigation edit using b10621 still needed substantive correction.
Human effort and time to accepted work were unmeasured.

### Structured extraction

Nine fictional cases checked complete records, associations, absence, Swedish
text, normalization and a legible invoice image against predefined answers.
The configuration was Unsloth Q4_K_M plus its BF16 projector, revision
`e87f176479d0855a907a41277aca2f8ee7a09523`, llama.cpp **b11381**, Metal,
8,192-token context, one slot, thinking off, Q8_0 KV cache, temperature 0 and
1,536 output tokens.

Ordinary chat returned the correct facts in all nine cases, but two answers
used a bare array instead of the required `records` envelope. A separate
JSON Schema condition constrained structure, field types and nullability
without supplying expected values or record counts. It produced valid
structure throughout and met the strict oracle in **8/9 cases**. The remaining
answer omitted a pending order: the question asked for a shipment record
although dispatch had not happened, while the oracle expected the order ID
with null shipping fields. That wording limits the failure's interpretation.

The invoice image passed in both conditions. These results make Qwen a useful
baseline before adding [NuExtract3](../specialists/nuextract3/README.md) solely
for extraction; they do not establish OCR quality on real scans or complex
documents. Grammar acceptance and factual correctness remain separate checks.

## Performance

Broader suite, measured **12 September 2026**.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10621 | 548 | 36.7 | 6.15 GiB |

Rates are medians across 41 model calls in 23 suite cases. Prefill measures
newly processed input with prompt caching enabled. Memory is the highest
sampled resident memory of the server process, not a minimum RAM requirement.

The b11381 structured-output configuration, measured **4 October 2026** on
the same M5/32 GiB and macOS 26.6.1, took a median **1.73 s** across eight text
requests and **3.71 s** for the invoice image. Peak sampled server RSS was
**5.07 GiB**, with no observed swap growth. These full-request times exclude
startup and include the pending-record omission; no large main model was
resident, and time to reviewed acceptance was not measured.

On b11149/v257, lookup across a long input and a follow-up both passed with
11,776 initial input tokens and a 4,096-token output allowance in the
16,384-token window. Measured 24 September 2026 on the same M5/32 GiB, the
configuration took 17.34 seconds for lookup and 17.02 seconds for the
follow-up, with 3.05 GiB peak sampled resident memory (RSS). This tests one
context length; it does not establish a maximum or fit on 8/16 GiB machines.

Saving one context checkpoint let the engine reuse most of the processed
history. A paired comparison on the same machine, engine and date measured:

| Checkpoints | Follow-up model service time | Reused / newly processed input tokens |
|---|---:|---:|
| 0 | 17.17–18.44 s | 0 / 11,880 |
| 1 | 1.85–1.88 s | 11,772 / 108 |

All sixteen answers were correct. One checkpoint used about 50–68 MiB more
sampled server RSS, with no observed swap growth. These are
forward-continuation results; rewind and branch behavior were not assessed.

[All models](../README.md).
