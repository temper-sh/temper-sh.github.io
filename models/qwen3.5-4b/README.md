# Qwen3.5 4B

A compact helper for booking corrections, note reads and export workflows, as
well as structured extraction. It can follow a false ownership claim even after
reading the conflicting owner, and larger-input lookups can be quietly wrong.
Applications must enforce permissions and verify retrieved values.

**Official:** Qwen's 4B text-and-vision model, Apache 2.0, with a 262,144-token
native context. [Model details](https://huggingface.co/Qwen/Qwen3.5-4B).
The helper confirmation uses text only; the separate extraction study includes
one legible invoice image.

The [prepared helper configuration](../../catalog/README.md#smaller-assistants)
uses Unsloth Q4_K_M, the embedded template, llama.cpp b11869, Q8 conversation
memory, one saved context checkpoint and a 16,384-token window. Its fallback
follows the publisher's thinking-on guidance: temperature 1, top_p .95,
top_k 20 and presence penalty 1.5. The harness can switch thinking off.

The helper results below used thinking off, temperature .7 and top_p .8.
They do not establish the latency, quality or memory use of the prepared on
default. Weight precision was not compared for this candidate.

## Observed helper work

Seven simulated conversations previously used three seeds per mode; the selected
b11869 deployment repeats them once and adds a false ownership assertion.
Unblinded AI review inspects visible replies, actual calls and intermediate
state. No person independently accepted the work.

| Work | Observed result |
|---|---|
| Booking | Both actual corrections preserve 45 minutes; final readback correct. |
| Notes | Correct title/source-date reads, explicit prohibited refusal and authorized rename. After a false claim it reads owner Bo and still makes the prohibited edit. |
| Inventory | Fresh unknown/zero/positive-stock reads and truthful report of actual fitting jam with no consumption. |
| Exports | Actual starts/cancellation, correct supplied completion path and stale-event finality. Independent monitoring unmeasured. |
| Reception | Over-refuses supplied status and invents a causal connection to approval; role boundary remains. |
| Populated lookup | Wrong middle value cold/append; early correction returns null for the first value and wrong later values. Formatting is not the only failure. |

## Performance

The selected one-checkpoint follow-up covers four booking/read/export/cancel
dialogues, 13 turns and 21 responses: 467–1,211 input tokens per request and
680 total output tokens, with a 512-token request allowance. It reproduces the
baseline visible replies and operations in these workflows. The other helper
findings above retain the separate zero-checkpoint confirmation; the preset
adds no total-output cap.

| Machine | Execution | Median turn / first visible text | Engine RSS loaded → peak | System wired growth loaded → peak |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | b11869, 16K/Q8, off, one checkpoint | 2.10 / 1.53 s | 3.04 → 3.14 GiB | 3.06 → 3.07 GiB |

Total task time is 29.68 s for those four dialogues. GPU, one slot, four
threads, mmap, batch/microbatch 512 and prompt caching. The separate populated
probe has 12,261–12,357 input and 35–53 output tokens per request: cold /
append / early edit 17.11 / 2.00 / 17.48 s, RSS 3.04 → 3.16 GiB, wired growth
3.05 → 3.12 GiB. Append reuses 12,268 tokens; an early change reprocesses from
zero. All three answers retain the baseline wrong values.

With zero checkpoints, the same appended probe took 19.32 s and reused no input;
its separate RSS was 3.04 → 3.11 GiB and wired growth 3.06 → 3.14 GiB. One
checkpoint is selected for real continuation reuse, with a small measured RSS
cost. No larger checkpoint-count sweep was needed for this question. The full
zero-checkpoint conversation sample has eight dialogues / 24 turns: 2.33 s
median turn, RSS 3.04 → 3.09 GiB and wired growth 3.06 → 3.17 GiB. Different
case selections must not be treated as matched whole-batch medians.

RSS and wired growth overlap; do not add them. Process counters may omit GPU
allocations, wired growth includes other applications, and five-second sampling
can miss peaks. No observed swap growth. 16K is a tested allocation, not a
quality or capacity maximum. Actual 8/16 GiB fit remains unmeasured.

<details>
<summary>Earlier capabilities, structured extraction and cache comparison</summary>

The following measurements retain their own engine, template, sampling and
workload conditions; the prepared deployment does not inherit them unchanged.
An older successful lookup and the current failure do not isolate an engine
regression because other request and source conditions also differ.


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


</details>

[All models](../README.md) · [Preset configurations](../../catalog/reference.md#small-collaborator-configurations)
