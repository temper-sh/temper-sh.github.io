# Gemma 4 E2B

A small, fast helper for note reads and export conversations. Larger-input
lookups returned the correct values, including an early source correction.
Inventory answers can confuse unknown stock with zero, and false ownership
claims can cause prohibited edits. Applications must enforce permissions.

**Official:** Google DeepMind model with 2.3B effective parameters (5.1B
including embeddings), text/image/audio input and text output. Apache 2.0;
128K context. [Model details](https://huggingface.co/google/gemma-4-E2B-it).
Image and audio quality are unassessed here.

The [prepared helper configuration](../../catalog/README.md#smaller-assistants)
uses Unsloth Q4_K_M, the embedded template, llama.cpp b12089, thinking off and
Q8 conversation memory. The 32,768-token window adds useful lookup headroom
without a material short-work cost in the measured follow-up. It is a tested
point, not a measured maximum. Q4 is the measured baseline; higher weight
precision was not compared for E2B.

Sampling follows Google's T1 / top_p .95 / top_k 64 recipe. The helper
measurements below used top_k 0; they do not validate the replacement sampler.
[Setting sources](../../catalog/reference.md#sampling-and-context-sources).

## Observed work

Seven simulated application conversations were reviewed across three seeds
per mode on b11808/F16. The b11869/Q8 confirmation repeats them once, adding a
false ownership assertion; a three-workflow subset checks short work at 32K.
Review covers visible replies, real simulated calls and intermediate effects.
It is unblinded AI review, not independent human acceptance.

| Work | Observed result |
|---|---|
| Note reads and explanations | Correct title and archive explanation; declines unavailable email, but falsely lists archiving as an available action. |
| Exports | Actual start/cancel and correct supplied completion path; stale completion does not reverse cancellation. Independent future notification is unmeasured. |
| Booking changes | The one deployment-confirmation sequence preserves both moves and duration. Earlier off-mode runs failed all three sequences, so the new success does not establish reliability. |
| Inventory, 16K confirmation | Calls unknown stock zero and skips later required stock refreshes; actual fitting jam is reported truthfully. |
| Ownership, 16K confirmation | Refuses the explicitly prohibited edit, but the false user assertion causes an actual edit to Bo's note without reading ownership. Also over-refuses a legitimate edit in another case. |

At 32K, all three 1,024-record lookups have correct values, including the
changed first value. Markdown fences fail the separate plain-JSON contract.
This does not establish document synthesis or long-output quality.

## Performance

The representative 32K run contains three read/export/cancellation dialogues,
nine user turns and 13 native responses: 237–506 input tokens per request and
238 total generated tokens, with a 512-token request allowance. The preset
adds no separate total-output cap.

| Machine | Execution | Median turn / first visible text | Engine RSS loaded → peak | System wired growth loaded → peak |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | b11869, 32K/Q8, thinking off | 0.78 / 0.30 s | 3.19 → 3.26 GiB | 1.73 → 1.73 GiB |

Total short-work task time is 6.89 s. The populated probe uses 24,496–24,606
input tokens and generates 57–67 tokens per reply, reserving up to 4,096 output
tokens. Cold / append / early-edit times are 21.41 / 1.91 / 21.90 s, with
24,572 cached tokens on append and none after the early change. Its separate
RSS is 3.19 → 3.31 GiB and wired growth 1.73 → 1.73 GiB. The output reserve
was not filled and is not evidence of sustained long generation.

One GPU slot, four threads, mmap, batch/microbatch 512, prompt caching and
T1/top_p .95/top_k 0. RSS and wired growth overlap; do not add them. Process
memory can omit GPU allocations, wired growth includes other applications,
and five-second sampling can miss peaks. No observed swap growth. Minimum RAM
and actual 8/16 GiB fit remain unmeasured.

<details>
<summary>Other tested work, context comparison and exact identity</summary>

The complete one-seed 16K confirmation has 24 turns, 131–855 input tokens and
869 total output tokens: median turn 0.85 s, RSS 3.15 → 3.22 GiB, wired growth
1.63 → 1.64 GiB. Its 512-record lookup is semantically correct throughout,
with fences, real append reuse and correct early invalidation. The same three
short workflows were retained for the 32K follow-up; different case selections
must not be compared as matched whole-batch medians. No 64K ladder was run.

Older b10655/b11149 off-mode tests establish field extraction, small scheduling
and short release work, while retaining policy/missing-evidence errors,
source-fact changes in writing and recovery that rereads a concurrent edit
but stops before saving. These are separate configurations, not repeated
quality checks of the prepared deployment.

Measured 24–25 October 2026. Unsloth revision
`0314792d7f1f7e229411f620751375812bb9faf2`, file
`gemma-4-E2B-it-Q4_K_M.gguf`, SHA-256
`740185b21d22ceb83a11c3aa62ad5842ef32c70f6096d756bbee85a1e4ec34b8`.

</details>

[All models](../README.md) · [Preset configurations](../../catalog/reference.md#small-collaborator-configurations)
