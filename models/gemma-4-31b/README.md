# Gemma 4 31B

**Our assessment:** Useful for creative drafting and revision, with much
longer waits than Gemma 26B on an M5 with 32 GiB. Scenes still needed factual
and continuity edits. Our small comparison did not establish a human
preference advantage over 26B, and operational advice needed independent checking.

**Official:** Google's dense 31B model accepts text and images, with a
262,144-token native context. Apache 2.0.
[Model details](https://huggingface.co/google/gemma-4-31B-it), checked
27 September 2026.

[Temper offers a Gemma 31B preset](../../catalog/README.md).
The measured configuration used Unsloth QAT UD-Q4_K_XL, the embedded template,
llama.cpp b11205, Q8 conversation memory, two context checkpoints, and a
40,960-token window. It used plain decoding and **thinking off**; Temper's
current medium-thinking default is a different setting.

## Writing and explanation

AI reviewers checked fixed English briefs. Human preference and time to an
accepted draft were not measured. Confirmation reused known cases.

| Work | Observed result |
|---|---|
| Scene and dialogue | Preserved the main facts, viewpoint, and unresolved tension; some emotional explanation could be trimmed. |
| Tone revision | Shortened and restrained the dialogue but omitted a supplied fact. |
| Archive scene, final settings | Met the main constraints; the physical explanation for hiding from a camera was unconvincing. |
| Laundromat scene, final settings | Kept a possible compromise and visible cost; timing and object continuity needed edits. |
| General explanation | A clear cache analogy that preserved the authoritative record and explained expiry and purge tradeoffs. |
| Operational advice | The proposed purge and spot check did not establish the required one-minute freshness guarantee. |

The Google QAT Q4_0 comparison also produced useful scenes. Unsloth's
[conversion-fidelity findings](https://unsloth.ai/docs/models/gemma-4/qat#qat-analysis)
do not establish better writing; our local comparison found no such advantage.

## Waiting time and memory

Measured 28 September 2026 on Apple M5 / 32 GiB / macOS 26.6.1, with
llama.cpp b11205 and llama-swap v260.

| Work / configuration | Time | Rate or memory |
|---|---:|---|
| Scene / revision / explanation, plain Q8 at 32k | 94.2 / 71.1 / 60.0 s | Different answer lengths |
| Archive / laundromat scenes, plain Q8 at 40,960 | 73.8 / 102.8 s | Answers still needed review |
| 512-token check, plain Q8 and checkpoints at 40,960 | 77.59 and 82.42 s | 6.67 and 6.29 tokens/s |
| Retrieve facts from 32,768 input tokens, same 40,960 setting | 583.4 s | Input processing: 58.00 tokens/s; peak engine RSS: 19.51 GiB |
| Follow-up using the same context | 17.3 s | Reused 32,764 tokens |

The 512-token check used greedy sampling, disabled prompt reuse, and forced
generation past normal stopping. At 32k, Unsloth took 76.50–76.79 seconds
versus Google's 78.62–78.69 seconds.

One-token MTP reduced median diagnostic time by 19% in the final paired
comparison, but introduced a central deadline error in the writing task.
That configuration was not selected. Its repeated timings varied for an
unresolved reason, and the results do not establish that MTP generally harms
writing quality.

## Long documents

The 40,960-token setting retrieved distributed facts and answered a follow-up,
with 4,096 tokens reserved for output. Both replies used Markdown JSON fences:
retrieval passed, strict bare-JSON formatting did not. Generation at this
filled-context point was 4.22 / 4.25 tokens/s.

No new swap or thermal warning was observed. Larger windows were not tested;
the first answer approached the ten-minute limit. The output reserve was not
fully generated. These are retrieval and continuation measurements, not a
general long-document assessment or minimum RAM requirement. Vision and
multilingual quality were not assessed.

[Compare Gemma 26B-A4B](../gemma-4-26b-a4b/README.md) · [All models](../README.md).
