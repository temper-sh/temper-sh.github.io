# Qwen3.8 27B / llama.cpp: technical reference

Document-work and context observations behind the [model overview](../README.md).
Results apply to the exact builds and conditions below. The
[Splash reference](splash-ud-q4-k-xl-reference.md) retains the matched coding
comparison and the separate Splash workloads.

Official model information on the overview was checked on 13 September 2026.

## What it completed

AI reviewers checked the document results, with simulated tool operations.
Human acceptance and repair effort were not measured. The coding comparison
used real tests and is retained in the linked Splash reference.

| Work | Tested configuration | Outcome |
|---|---|---|
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

Apple M5 / 32 GiB / macOS 26.6.1; output counts include reasoning.

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
long-document reasoning. [Context and conversation-cache guidance](../tuning.md)
explains the practical settings.

[Model overview](../README.md) · [Technical references](../../../guides/technical-reference.md).
