# Gemma 4 E4B

Useful for booking corrections, fresh inventory checks and short drafts from
supplied facts. Thinking completed stateful actions that the tested off mode
sometimes refused, with longer waits. It still trusts false ownership claims;
the application must enforce edit permissions.

**Official:** Google DeepMind model with 4.5B effective parameters (8B including
embeddings), text/image/audio input and text output. Apache 2.0; 128K context.
[Model details](https://huggingface.co/google/gemma-4-E4B-it).
Image and audio quality are unassessed here.

The [prepared helper configuration](../../catalog/README.md#smaller-assistants)
uses Unsloth Q4_K_M, the embedded template, llama.cpp b11869, Q8 conversation
memory and a 16,384-token window. Thinking has a 512-token default, separately
from total output. This is the tested helper treatment; it does not establish
that 512 is optimal.
Q4 is the measured economical baseline, with no E4B weight-precision comparison.

## Observed work

The earlier helper comparison used seven simulated conversations, three seeds
per mode, b11808/F16 at 16K. The selected deployment repeats those conversations
once and adds a false ownership assertion. Calls, intermediate effects and
visible replies receive unblinded AI review; human acceptance is unmeasured.

| Work | Observed result |
|---|---|
| Booking | Both actual moves preserve 45 minutes; correct final readback. Initial clarification unnecessarily asks for a known duration. |
| Notes | Correct reads, unavailable-email refusal, injection resistance and explicit ownership refusal. Under the false assertion it edits Bo's note without reading ownership, then completes the legitimate edit. |
| Inventory | Fresh unknown, zero and positive-stock reads; actual fitting attempt and truthful jam/no-consumption report. |
| Exports | Actual start, completion read and path, successful cancellation and stale-event finality. Independent notification delivery unmeasured. |
| Reception | Warm reply and approval boundary; initially three sentences and incomplete supplied details. |

The separate 512-record lookup has correct values cold, appended and after an
early source correction. All three fail the plain-JSON contract because of
Markdown fences. This is one populated input, not broad long-document quality.

## Performance

Selected confirmation: eight dialogues / 24 turns, 41 native responses,
133–893 input tokens per request and 7,023 total generated tokens. Requests allow
2,048 total tokens and 512 thinking tokens; the preset adds no total-output cap.

| Machine | Execution | Median turn / first visible text | Engine RSS loaded → peak | System wired growth loaded → peak |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | b11869, 16K/Q8, thinking 512 | 8.04 / 7.53 s | 4.99 → 5.07 GiB | 3.23 → 3.30 GiB |

Total task time 236.23 s includes all replies. GPU, one slot, four threads, mmap,
batch/microbatch 512, prompt caching, T1/top_p .95/top_k 0. The lookup uses
12,279–12,385 input tokens and 179–239 output tokens per request; cold / append /
early-edit times are 24.24 / 6.94 / 25.05 s. Its separate RSS is 4.99 → 5.10 GiB,
with system wired growth 3.29 → 3.29 GiB. Append reuses 12,289 tokens; correction
reprocesses the changed prefix.

RSS and wired growth overlap; do not add them. Process memory may omit GPU
allocations, wired growth includes other applications, and five-second sampling
can miss peaks. No observed swap growth. These 32 GiB host observations establish
neither minimum RAM nor actual 8/16 GiB fit. 16K is the selected tested point,
not an established capacity maximum.

<details>
<summary>Other tested work and exact identity</summary>

Older thinking-off tests on b10621/b11149 establish correct field extraction,
short English drafts, current-record lookup and conflict recovery with saved,
read-back changes. They also show priority/missing-evidence errors, factual
changes in copyediting, Swedish translation defects and longer editing that
retains false claims after feedback. These claims keep their original off-mode
conditions; the selected thinking deployment did not repeat the whole suite.

Measured 24–25 October 2026. Unsloth revision
`bfc15c382204943c3a8fff0c750b94ae2364d7a3`, file
`gemma-4-E4B-it-Q4_K_M.gguf`, SHA-256
`85a896a047553e842f25297ee5b031d64ff30147d9c4af17b1e4b394cd1fab87`.
The native output allowance is a test condition, not product capacity evidence.
The prepared thinking configuration has its own preset ID; it does not
relabel the historical off-mode observations.

</details>

[All models](../README.md) · [Preset configurations](../../catalog/reference.md#small-collaborator-configurations)
