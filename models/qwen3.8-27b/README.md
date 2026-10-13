# Qwen3.8 27B

**Our assessment:** Useful for coding, combining sources, and revising documents.
On the tested M5 with 32 GiB, Splash produced first coding patches much sooner
than llama.cpp. Both needed code review and corrections. Document work with
llama.cpp was useful, while strict output formats and Swedish prose needed checking.

**Official:** Qwen's dense 27B model accepts text, images, and video, with a
262,144-token native context. Apache 2.0.
[Model details](https://huggingface.co/Qwen/Qwen3.8-27B), checked
13 September 2026.

[Temper offers Splash and llama.cpp presets](../../catalog/README.md).
It installs the engine and prepares the model files. The coding measurements
below used an earlier Splash source build; they are not measurements of the
current packaged release. See the [coding comparison](layouts/splash-ud-q4-k-xl.md)
for the exact setup and limitations.

## What it completed

AI reviewers checked the results. Document workflows used simulated tools;
coding patches ran against real tests. Human acceptance and repair effort
were not measured, and the Splash code review knew which engine produced each patch.

| Work | Tested configuration | Outcome |
|---|---|---|
| Streaming bug fix and decorator feature | Splash fdc11c5, Unsloth UD-Q4_K_XL, Frog v22.5, DFlash2 | Passed the required behavioral checks; review found overly broad error suppression and an unnecessary API expansion. |
| Registration, handover, and notice revision | llama.cpp b10964 | Completed all four jobs reserved for validation, including a protected quotation and an authoritative venue change. |
| Combining sources and recovering edits | llama.cpp b10964 | Completed 7/10 jobs; remaining problems included a misplaced JSON field, a small counting error, and Swedish prose needing broader revision. |
| Copyediting protected facts | llama.cpp b10936 | 15/18 jobs accepted by the AI review; presentation and output-format corrections remained. |
| Changed instructions | llama.cpp b10936 | All 12 followed the latest rules; one still needed a closing quote removed. |
| Longer tool workflows | llama.cpp b10936 | Procurement completed; notice recovery missed an appointment, and a Ruby plan made incorrect repository claims. |
| Multilingual work | llama.cpp b10936 | English and Spanish needed corrections; Swedish needed broader revision; Ukrainian used its output budget without delivering the notice. |
| Five focused checks | llama.cpp b11149 | Extraction and updated instructions worked; a copyedit changed a protected plural, and a recovered edit was reported with the wrong tool-call count. |

The llama.cpp document measurements used the Frog v22.5 template and
UD-Q4_K_XL weights. Results from different engine versions and workloads
remain separate.

## Waiting time and memory

All figures below are from Apple M5 / 32 GiB / macOS 26.6.1.
Output counts include reasoning.

| Coding task, 25–26 September 2026 | Splash fdc11c5 | llama.cpp b11157 |
|---|---:|---:|
| First streaming-fix submission | 40m 27s | 102m 21s |
| First decorator-feature submission | 19m 21s | 58m 23s |

The engines used the same target weights and Frog template, with different
drafting mechanisms. These are times to first submission, excluding startup
and subsequent repairs. They do not establish time to accepted code.

For document/source work on b10964 at 32k, per-request medians were
107.18 newly processed input tokens/s and 8.68 generated tokens/s, with
21.87 GiB peak engine RSS (15 September 2026).

## Long documents with llama.cpp

The 24 September capacity test used b11157 / llama-swap v257, UD-Q4_K_XL,
Frog v22.5, medium thinking, three-token MTP, Q8 conversation memory,
14 checkpoints, 2 GiB of extra cache RAM, and a 40,960-token window.

| Request | Newly processed input | Output | Input processing | Complete response |
|---|---:|---:|---:|---:|
| Initial fact lookup | 36,352 tokens | 232 tokens | 456–474 s | 488–506 s |
| Follow-up | 94 tokens | 129 tokens | 3.24–3.26 s | 21.0–21.1 s |
| Forced full-output check | 132 tokens | 4,096 tokens | 3.62 s | 535.64 s |

The lookups passed twice in fresh sessions. The follow-up reused 36,348 tokens;
peak sampled engine RSS was 20.83 GiB with no swap growth or pressure warning.
The forced-output check disabled normal stopping and tested resource use,
not answer quality.

Larger short-answer lookups succeeded, but subsequent follow-ups or full-output
requests triggered the memory-pressure guard. The cause remained unresolved;
no model crash was observed. An earlier b10964 test handled about 100k input
with only 512 output tokens, taking roughly 37 minutes and adding 87.7 MiB of
swap. It does not establish that size with a full answer budget.

These tests do not establish a maximum context, minimum machine RAM, or general
long-document reasoning. [Context and conversation-cache guidance](tuning.md)
explains the practical settings.

[All models](../README.md) · [Choose a model](../../guides/choose-a-local-model.md).
