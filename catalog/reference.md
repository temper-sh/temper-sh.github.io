# Preset configuration and measurement reference

Details behind the [preset directory](README.md), for readers and agents who
need exact conditions and evidence limits. These observations include evaluated
variants beyond the published catalog. They do not establish general model
quality, minimum machine RAM or performance for different settings.

## Published settings and evaluated variants

Temper resolves installation inputs from the signed catalog. The
[stable channel](v1/channels/stable/channel.yaml) identifies its snapshot.
[Authoring sources](sources/apple-silicon.json) may contain unpublished changes.
See [the reference reading guide](../guides/technical-reference.md) before using
an experiment's settings to prepare a setup.

| Configuration | Published preset | Additional evaluated configuration |
|---|---|---|
| Qwen / Splash | 112,000-token window for 32 GiB+ | 131,072-token profile for 64 GiB |
| Qwen / Kapitsa | MLX4/DFlash2, 98,304-token window, thinking off | Larger prefill chunks and alternative contexts are retained in the [reference](../models/qwen3.8-27b/layouts/kapitsa-mlx4-reference.md) |
| Muse Glimmer | DFlash2 at 131,072 tokens, or plain decoding at 57,344 | Matched plain/DFlash2 comparison at 57,344 tokens |
| Gemma 26B | Plain decoding, 131,072-token window and a 1,024-token reasoning budget | Earlier 98,304-token configuration and draft experiments |
| Gemma 31B | MTP width 3, 40,960-token window and a 1,024-token reasoning budget | Plain decoding, other MTP widths and draft methods |

The measurements below describe the named executions. Kapitsa and profiles
containing memory observations require Temper 0.4.0 or newer. Temper checks
each preset's compatibility before offering it; the signed catalog owns those
requirements.

## Response and reasoning settings

Chat presets let responses use the context remaining after the input, including
reasoning. They impose no separate output-token cap. Your app can request a
smaller allowance; a normal completion can end before either limit. The context
settings and memory allowances still apply.
Measurements retain their stated request budgets; an output allowance alone
does not establish completed-work quality.

The evaluated Gemma configurations below allow up to 1,024 thinking tokens
before the answer. This controls reasoning separately from the total response allowance. Apps can
override it with `reasoning_budget_tokens` (`-1` for unrestricted thinking,
`0` to end thinking immediately, or a positive token count).

## Qwen / Splash

The [Splash technical reference](../models/qwen3.8-27b/layouts/splash-ud-q4-k-xl-reference.md)
owns the configuration comparison, coding review findings and workload details.
The memory table below retains the observations referenced by catalog records.

## Muse Glimmer configurations

The evaluated Muse DFlash2 configuration uses llama.cpp b11741, a seven-token
draft, medium thinking, F16 conversation memory and a 131,072-token window.
On M5 / 32 GiB it passed
retrieval from 122,880 input tokens, a source correction, a full 4,096-token
generation and another lookup. Engine RSS was 18.38 GiB loaded → 19.62 GiB peak,
with no swap. The cold lookup took 25.7 minutes; its cached follow-up took
29.7 seconds. These checks establish capacity and cache behavior, not general
long-document accuracy.

In the earlier matched 57,344-window comparison, short 512-token probes took
33.5 s median with DFlash2 versus 64.6 s plain. Engine RSS loaded/peak was
17.47/17.72 GiB versus 15.79/15.82 GiB. Both modes confused values in a dense
directory and invented an unsupported publication condition in an announcement.
The [Muse assessment](../models/muse-glimmer-30b/README.md) gives the methods
and limits. The plain Muse preset retains its 57,344-token window.
Both configurations use the same target weights.

## Gemma writing configurations

The evaluated Gemma 26B configuration uses plain decoding with a 131,072-token
window. The evaluated Gemma 31B configuration uses a
small model-matched draft predictor (MTP width 3) at 40,960 tokens. Both use
Unsloth QAT UD-Q4_K_XL weights, llama.cpp b11756, thinking on and the 1,024-token
reasoning default. The 31B sidecar adds about 280 MB of model files.

On Apple M5 / 32 GiB, the 20 October 2026 five-task writing comparison preserved
the tested facts and chronology in all five tasks for both choices. At a 16,384-
token allocation, 26B plain took 143.87 seconds, with 14.18 → 14.53 GiB engine
RSS; 31B MTP-3 took 565.21 seconds, with 17.95 → 18.73 GiB. Plain 31B took
1,143.71 seconds, with 17.60 → 18.35 GiB. The MTP-3 peak covers all ten
thinking-off/on requests; timings cover only the five thinking-on tasks.
Outputs differed in length, and length/coverage defects remained. These are
completed-response times, not time to human acceptance or a creative-preference
ranking. Width 7 was faster on short tasks but slower than plain decoding in
the matched populated-context check, so the evaluated configuration uses width 3.

At the selected windows, both configurations passed ledger retrieval, appended
lookup, early correction, full 4,096-token generation and final recall with
thinking off. Cold input was 122,880 tokens for 26B and 32,768 for 31B; final
recall used 126,969 and 37,048 input tokens. Engine RSS was respectively
16.37 → 17.35 GiB and 18.95 → 19.97 GiB, with no swap growth. Cold lookup took
510.48 / 432.18 seconds and cached follow-up 5.52 / 11.38 seconds. Responses
had correct values inside unwanted JSON fences. Capacity and cache behavior
do not establish general long-document reasoning quality.

A reserved short writing brief at these windows passed the factual and
140–180-word requirements with thinking on and the 1,024-token budget: 26B
took 27.92 seconds (16.37 → 16.44 GiB RSS), and 31B MTP-3 took 93.20 seconds
(18.95 → 19.13 GiB). The catalog records the larger capacity-workload peaks for
memory accounting. All these tests used llama-swap v265 and macOS 26.6.1,
one resident model and two-second memory sampling. They do not isolate an
engine-upgrade effect or establish vision, tool-use or multilingual quality.

## Memory observations

Temper 0.4.0 uses an applicable catalog measurement before estimating memory. These
observations match the listed preset settings on Apple M5 / 32 GiB, macOS
26.6.1, with a 24 GiB Metal budget, except the explicitly named M5 Pro / 64 GiB
row (macOS 26.7.1, 53,084 MiB Metal budget). Changing the engine, weights, runtime
settings or machine makes Temper use estimation until a matching measurement
is available.

| Preset and tested engine | Date | Loaded → peak | Measured workload |
|---|---|---|---|
| Qwen/Splash 1.3.2, 112,000-token window | 2026-10-17 | 19.46 → 20.46 GiB engine physical footprint | Six short interface/reuse requests, largest input 522 tokens. Loaded after the first response. |
| Qwen/Splash 1.3.2, M5 Pro / 64 GiB, 131,072-token window | 2026-10-20 | 19.22 → 21.63 GiB system wired-memory growth | Two-response coding workflow, thinking on; 16,369 input + 43,375 generated tokens in the first response, then 20,846 input + 4,018 generated. Loaded after readiness. Submitted code failed independent tests; resource measurements remain applicable. |
| Muse plain, llama.cpp b11741, 57,344-token window | 2026-10-19 | 15.79 → 16.13 GiB engine RSS | 49,152-token ledger retrieval, appended lookup, source correction, 512 generated tokens and follow-up. |
| Muse DFlash2, llama.cpp b11741, 131,072-token window | 2026-10-20 | 18.38 → 19.62 GiB engine RSS | 122,880-token ledger retrieval, appended lookup, source correction, 4,096 generated tokens and follow-up. |
| Gemma 26B plain, llama.cpp b11756, 131,072-token window | 2026-10-20 | 16.37 → 17.35 GiB engine RSS | Thinking-off capacity sequence: 122,880-input ledger, appended lookup, correction, 4,096 generated tokens and 126,969-input final recall. Loaded after readiness. |
| Gemma 31B MTP-3, llama.cpp b11756, 40,960-token window | 2026-10-20 | 18.95 → 19.97 GiB engine RSS | Thinking-off capacity sequence: 32,768-input ledger, appended lookup, correction, 4,096 generated tokens and 37,048-input final recall. Loaded after readiness. |
| TranslateGemma 4B, llama.cpp b11716, 4,096-token window | 2026-10-17 | Not sampled → 3.01 GiB engine RSS | Two short French/German translations, each with 84 input and 14 output tokens. |

These are observed peaks, not worst-case bounds or minimum machine RAM. System
wired-memory growth includes other applications; approximately five-second
sampling on the M5 Pro can miss short peaks. The engine counters exclude the
router and, for Splash, its frontend. Splash RSS
and the tested llama.cpp physical-footprint counter omit much of the mapped
model memory, so the table uses different counters and does not add them to
overlapping system wired memory. No swap growth was observed. Short requests
do not establish full-window use; the larger Muse and Gemma workloads test capacity and
reuse, not general long-document accuracy. Configurations without applicable observations use estimates:
older-engine measurements and unmeasured configurations are not interchangeable.

## Specialist configurations and observations

| Preset | Model license | Input and role | Engine and resource tradeoff |
|---|---|---|---|
| NuExtract3 4B | [Apache-2.0][apache-license] | Text or document image → structured records | llama.cpp, Metal by default, Q4_K_M plus BF16 projector; 3.46 GB of weight files. Peak server RSS: 3.69 GiB GPU / 6.31 GiB CPU. The tested image took 3 s on GPU versus 145 s on CPU. |
| Ettin Reranker 150M | [Apache-2.0][apache-license] | English query and candidate passages → ranked candidates | CrispEmbed, CPU/four threads, Q8_0; 162 MB of weights. The 20-candidate sample took 0.76 s median at 194 MiB peak RSS. |
| Qwen3 Reranker 0.6B | [Apache-2.0][apache-license] | Multilingual query and passages → ranked candidates | llama.cpp, CPU by default with GPU available, Q8_0; 639 MB of weights. CPU: 4.27 s median at 2.47 GiB RSS. GPU: 0.79 s at 1.32 GiB RSS. |
| TranslateGemma 4B | [Gemma][gemma-license] | Text translation with explicit language codes | llama.cpp/Metal, Q5_K_M; 2.83 GB of weights, 3.00 GiB peak RSS. Short requests: 0.77–1.48 s. The tested UI translation merged Discard and Cancel. |
| TranslateGemma 12B | [Gemma][gemma-license] | Text translation with explicit language codes | llama.cpp/Metal, Q5_K_M; 8.45 GB of weights, 8.66 GiB peak RSS. Short requests: 2.36–4.19 s. Kept UI actions distinct, but changed timing and negation elsewhere. |
| TranslateGemma 27B | [Gemma][gemma-license] | Text translation with explicit language codes | llama.cpp/Metal, Q4_K_M; 16.55 GB of weights, 16.39 GiB peak RSS. Short requests: 4.46–8.55 s. Preserved the five short cases' reviewed meaning; changed operational conditions in the longer document. |
| Tiny Aya Global | [CC BY-NC 4.0 · Noncommercial][noncommercial-license] | Translation across mixed language regions; balanced multilingual choice | Official Q8_0, llama.cpp/Metal; 3.57 GB of weights, 3.78 GiB peak RSS. Short requests: 1.06–1.88 s. Lost some operational meaning in the local sample. |
| Tiny Aya Water | [CC BY-NC 4.0 · Noncommercial][noncommercial-license] | European and Asia-Pacific languages, including Swedish, German, French, Chinese, Japanese and Korean | Official Q8_0, llama.cpp/Metal; 3.57 GB of weights, 3.79 GiB peak RSS. Short requests: 1.06–1.89 s. Short meanings held with awkward UI labels; the longer procedure had errors. |
| Tiny Aya Fire | [CC BY-NC 4.0 · Noncommercial][noncommercial-license] | South Asian languages, including Hindi, Bengali, Gujarati, Marathi, Tamil, Telugu and Nepali | Official Q8_0, llama.cpp/Metal; 3.57 GB of weights. Regional coverage follows Cohere; local language quality and comparable performance are unmeasured. |
| Tiny Aya Earth | [CC BY-NC 4.0 · Noncommercial][noncommercial-license] | West Asian and African languages, including Arabic, Persian, Turkish, Hebrew, Swahili, Amharic, Hausa and Yoruba | Official Q8_0, llama.cpp/Metal; 3.57 GB of weights. Regional coverage follows Cohere. One Swahili check changed Tuesday to Monday; wider quality and comparable performance are unmeasured. |

Ettin's native SentenceTransformers route was faster in the same comparison
(0.39 s median) but used about 1.02 GiB RSS. The selected CrispEmbed conversion
retained retrieval quality on that 50-query sample with a smaller footprint.
It has a fixed 7,999-token query/passage limit. Split longer documents before
reranking. Qwen also accepts multilingual inputs; the linked card states the tested scope.

TranslateGemma's publisher reports support for 55 languages and a 2K-token
input limit. The presets use a 4,096-token total window, greedy decoding and
F16 KV, and disable speculation. The embedded template and llama.cpp's native
language-code handling preserve the publisher's translation prompt. A six-case check
across English, Swedish, German and French found meaning errors in all three
sizes; it is model-assisted review, not an independent bilingual assessment.
The longer document took 20, 59 and 116 seconds respectively; 27B required a
retry after a Temper supervision interruption. CPU translation performance and
the wider language set are unmeasured. Published full-precision quality does
not measure these quantizations.

Tiny Aya's four instruction-tuned variants emphasize different regions within
the family's advertised 70+ languages. [Cohere's model cards](https://huggingface.co/CohereLabs/tiny-aya-global#model-details)
provide the language list and regional descriptions. All four carry noncommercial terms. They take a normal chat
instruction naming both languages and use their embedded template. The presets
select the official Q8_0 builds, GPU execution, a 4,096-token
total window, greedy decoding, F16 KV, one slot and batch 512/microbatch 256.
This is the measured Global/Water baseline, not a claim of optimal family tuning.
The catalog allows 8,192 total tokens against the publisher's 8K input guidance;
local capacity above 4K and CPU performance are unmeasured. Global/Water's
six-case results do not transfer to Fire/Earth or the wider language set.

The [specialist assessments](../models/specialists/README.md)
compare choices within each task, including meaning errors and evidence limits.

The [specialist request guide](https://github.com/temper-sh/temper/blob/master/docs/SPECIALISTS.md) shows the different
request contracts. Native measurements above are from M5/32 GiB on 2026-10-04,
using llama.cpp b11396 or CrispEmbed 0.17.11; they establish
component costs, not minimum machine RAM or performance alongside a main model.


[Explore presets](README.md) · [Technical reference guide](../guides/technical-reference.md)

[apache-license]: https://choosealicense.com/licenses/apache-2.0/
[gemma-license]: https://ai.google.dev/gemma/terms
[noncommercial-license]: https://creativecommons.org/licenses/by-nc/4.0/
