# Gemma 4 26B-A4B

**Our assessment:** A practical starting point for creative drafting, revision,
and general explanations on an M5 with 32 GiB. It answered much sooner than
Gemma 31B in our comparison. Facts, continuity, and constraints still needed
editing, including a scene whose proposed compromise could not fit before closing.

**Official:** Google's mixture-of-experts model has 26B total parameters and
uses about 4B per token. It accepts text and images, with a 262,144-token native
context. Apache 2.0.
[Model details](https://huggingface.co/google/gemma-4-26B-A4B-it), checked
27 September 2026.

[Temper offers a Gemma 26B preset](../../catalog/README.md).
The measurements below used Unsloth QAT UD-Q4_K_XL, the embedded template,
llama.cpp b11205, and **thinking off**. Temper's current medium-thinking
default is a different setting.

## Writing and explanation

AI reviewers checked fixed English briefs. Most tasks used Q8 conversation
memory at 32k context; the final archive and laundromat scenes also used F16,
two context checkpoints, and a 98,304-token window. Human preference and time
to an accepted draft were not measured.

| Work | Observed result |
|---|---|
| Scene and dialogue | Preserved viewpoint and the central tension, but moved an object inconsistently. |
| Tone revision | Shortened and restrained the dialogue; omitted a supplied fact and retained the continuity error. |
| Archive scene, final settings | Met the brief, including dialogue, visible extra time, and an unresolved dispute. |
| Laundromat scene, final settings | Proposed a compromise that exceeded the closing-time constraint and needed rewriting. |
| General explanation | A useful cache analogy with an unsupported claim about inevitable database crashes. |
| Operational advice | Suggested checks did not establish the required one-minute freshness guarantee. |

The 32k versions of the two additional scenes had different successes and
failures. A change in settings did not consistently improve writing.

Unsloth's 14.25 GB file was slightly smaller than Google's 14.44 GB QAT Q4_0.
The local comparison found mixed writing outcomes; the publisher's
[conversion analysis](https://unsloth.ai/docs/models/gemma-4/qat#qat-analysis)
does not establish a literary-quality improvement.

## Waiting time and memory

Measured 28 September 2026 on Apple M5 / 32 GiB / macOS 26.6.1, with
llama.cpp b11205 and llama-swap v260.

| Work / configuration | Time | Rate or memory |
|---|---:|---|
| Scene / revision / explanation, plain Q8 at 32k | 16.0 / 11.3 / 9.5 s | Different answer lengths |
| Archive / laundromat scenes, plain F16 at 98,304 | 11.6 / 14.0 s | 47.49 / 46.89 tokens/s |
| 512-token check, Unsloth plain Q8 at 32k | 11.30 and 11.40 s | 45.88 and 45.48 tokens/s |
| Retrieve facts from 90,112 input tokens, plain F16 at 98,304 | 554.3 s | Input processing: 163.93 tokens/s; peak engine RSS: 16.53 GiB |
| Follow-up using the same context | 5.0 s | Reused 90,108 tokens |

The 512-token check used greedy sampling, disabled prompt reuse, and forced
generation past normal stopping. Unsloth's median request time was about 5%
lower than Google's in that matched check.

A faster configuration using one-token MTP reduced diagnostic time by about
16%, but failed the writing check. Other settings also changed in that
comparison, so it does not isolate MTP's effect. Plain decoding retained a
closing-time failure too. Fast completion was not time to an accepted draft.

## Long documents

The 98,304-token setting correctly retrieved distributed records and answered
a follow-up, with 4,096 tokens reserved for output. The replies used Markdown
JSON fences, so they failed the stricter bare-JSON requirement. Generation at
this filled-context point was 17.14 / 17.16 tokens/s.

No new swap or thermal warning was observed. Larger windows were not tested;
the first answer already approached the ten-minute limit. The output reserve
was not fully generated. These measurements establish retrieval and
continuation, not general long-document reasoning or minimum machine RAM.
Vision and multilingual quality were not assessed.

[Compare Gemma 31B](../gemma-4-31b/README.md) · [All models](../README.md).
