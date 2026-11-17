# Gemma 4 26B-A4B

**Our assessment:** QAT Q4 produced useful drafts, revisions and explanations,
but facts, chronology and length constraints still needed editing. Longer
thinking did not consistently repair those defects. On an M5 Pro / 64 GiB,
three-record retrieval succeeded around 49k input tokens and was inconsistent
around 98k despite healthy memory. These observations do not establish general
long-document reliability.

**Official:** Google's mixture-of-experts model has 26B total parameters and
uses about 4B per token. It accepts text and images, with a 262,144-token native
context. Apache 2.0.
[Model details](https://huggingface.co/google/gemma-4-26B-A4B-it).

[Temper offers a Gemma 26B preset](../../catalog/README.md).
The measurements used Unsloth QAT UD-Q4_K_XL, the embedded
template and plain decoding. M5 / 32 GiB measurements used llama.cpp b11205
with **thinking off**. M5 Pro / 64 GiB measurements used locked llama.cpp
b12089 with **thinking on**, F16 conversation memory, one slot and a
131,072-token allocation. The latter fixed temperature 1, top-p 0.95,
top-k 64 and neutral penalties. These configurations are separate evidence.

## Writing and explanation

For the M5 thinking-off comparison, AI reviewers checked fixed English briefs.
Most tasks used Q8 conversation memory at 32k context; the final archive and
laundromat scenes also used F16, two context checkpoints, and a 98,304-token
window. Human preference and time to an accepted draft were not measured.

| Work, M5 / thinking off | Observed result |
|---|---|
| Scene and dialogue | Preserved viewpoint and the central tension, but moved an object inconsistently. |
| Tone revision | Shortened and restrained the dialogue; omitted a supplied fact and retained the continuity error. |
| Archive scene, final settings | Met the brief, including dialogue, visible extra time, and an unresolved dispute. |
| Laundromat scene, final settings | Proposed a compromise that exceeded the closing-time constraint and needed rewriting. |
| General explanation | A useful cache analogy with an unsupported claim about inevitable database crashes. |
| Operational advice | Suggested checks did not establish the required one-minute freshness guarantee. |

The 32k versions of the two additional scenes had different successes and
failures. A change in settings did not consistently improve writing.

For the M5 Pro thinking-on comparison, the same notice task took 14.82 seconds
with a 1,024-token reasoning budget, 52.93 seconds with 4,096, and 91.30 seconds
with unrestricted thinking. Loaded → peak system wired growth was respectively
17.26 → 17.26, 17.16 → 17.36 and 17.15 → 17.30 GiB. The shortest budget
produced two notices below the requested 65–90 words. Unrestricted thinking
met both word counts but added unsupported maintenance rationale; the 4,096
budget still left one notice short. All had 16,384 total output tokens
available and completed below that cap.

On a chronology scene, both 4,096 and unrestricted thinking identified the
feasible bus but left an unexplained hour of delay and did not establish the
advance to closing time before locking doors. A separate scene/revision pair
preserved the supplied schedule: the initial scene missed its minimum length,
while the revision met its length requirement. These single-task comparisons
do not establish an optimum reasoning budget or a clean overall quality pass.

Unsloth's 14.25 GB file was slightly smaller than Google's 14.44 GB QAT Q4_0.
The local comparison found mixed writing outcomes; the publisher's
[conversion analysis](https://unsloth.ai/docs/models/gemma-4/qat#qat-analysis)
does not establish a literary-quality improvement.

## Waiting time and memory

Representative retrieval sequences; service times include thinking and final
answer generation. The inputs and engine/settings differ between machines,
so these rows are not a hardware speed comparison.

| Machine / configuration | Work and result | Service time | Memory, loaded → peak |
|---|---|---|---|
| M5 / 32 GiB, b11205, thinking off, F16 at 98,304 | 90,112-input retrieval and follow-up semantically correct | Cold 554.3 s; follow-up 5.0 s | Engine RSS: loaded not retained in this summary → 16.53 GiB |
| M5 Pro / 64 GiB, b12089, thinking on, F16 at 131,072 | 49,012-input retrieval, append and early edit semantically correct; bare-JSON formatting failed | Cold 59.07 s; append 8.19 s; early edit 55.28 s | System wired growth: 17.11 → 17.46 GiB |

RSS and system wired growth are different counters. Wired growth includes other
applications, and its five-second sampling can miss peaks. Process counters can
omit mapped/GPU allocations; do not sum them with wired growth. Neither counter
establishes minimum machine RAM. The listed sequences observed zero swap growth.

On the M5, scene / revision / explanation at Q8 32k took 16.0 / 11.3 / 9.5
seconds with different answer lengths. Archive / laundromat scenes at F16
98,304 took 11.6 / 14.0 seconds (47.49 / 46.89 tokens/s). A 512-token Q8 32k
check took 11.30 and 11.40 seconds (45.88 and 45.48 tokens/s).

The 512-token check used greedy sampling, disabled prompt reuse, and forced
generation past normal stopping. Unsloth's median request time was about 5%
lower than Google's in that matched check.

A faster configuration using one-token MTP reduced diagnostic time by about
16%, but failed the writing check. Other settings also changed in that
comparison, so it does not isolate MTP's effect. Plain decoding retained a
closing-time failure too. Fast completion was not time to an accepted draft.
Loaded/peak memory for these matched speed checks is not retained in this summary.

## Long documents

The M5 thinking-off 98,304-token setting correctly retrieved distributed
records and answered a follow-up, with 4,096 tokens reserved for output.
The replies used Markdown JSON fences, so they failed the stricter bare-JSON
requirement. Generation at this filled-context point was 17.14 / 17.16 tokens/s.

No new swap or thermal warning was observed in that sequence. Larger windows
were not tested in that comparison; the first answer already approached the
ten-minute limit. The output reserve was not fully generated. These measurements
establish retrieval and
continuation, not general long-document reasoning or minimum machine RAM.

The M5 Pro thinking-on test asked for three values in generated record material.
Around 49k input tokens, cold, append and early-edit answers were all correct
after removing outer Markdown fences. Append reused 49,012 tokens; early edit
reused none. The 1,024-token total output allowance was sufficient for these
three responses.

At 97,886 cold-input tokens, the final adequately provisioned sequence answered
cold correctly but returned the wrong middle record after both append and an
early edit. Service times were 162.00 / 32.15 / 161.11 seconds; append reused
97,886 tokens. Loaded → peak system wired growth was 17.30 → 18.02 GiB,
with zero swap growth and no resource stop. Each request allowed 4,096 total
output tokens and completed, including the 1,093-token append response.

Three responses in preceding 98k attempts exhausted a 1,024-token total output
allowance without a final answer. Their semantic correctness is unassessed;
they are completion failures, separate from completed wrong answers. Formatting
failures are also separate from semantic correctness. These observations show
inconsistent retrieval under the tested conditions, not a physical context
ceiling or a demonstrated cache defect. Different input material and settings
prevent interpreting the M5 and M5 Pro outcomes as a regression.

Vision and multilingual quality were not assessed in these comparisons.

<details>
<summary>Measurement provenance</summary>

M5 measurements: 28 September 2026, macOS 26.6.1, llama.cpp b11205 and
llama-swap v260. Official model facts checked 27 September 2026.

M5 Pro measurements: 5 November 2026, macOS 26.7.1, Temper 0.5.0, Field Kit
0.8.0-dev, llama-swap v269. All ten execution locks select llama.cpp b12089
(`9309bf1bcfe8d52b0fb7ef906474db94041a5e0c`); the report's b11756 label was
an attribution error referring to inherited catalog memory evidence. An
independent engine version banner was not retained. Weights SHA-256:
`a7c5bc715f5ff8e99a3e8901ce7d2b42b402c669bf24f7c5250747633d0f5891`.
The 49k and final 98k sequences are runs `20261105T185718262229Z` and
`20261105T192000576366Z` in the returned `gemma26-q4-first` study.

</details>

[Compare Gemma 31B](../gemma-4-31b/README.md) · [All models](../README.md).
