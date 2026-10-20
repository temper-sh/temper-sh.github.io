# Choose a preset

Temper pairs each model with weights chosen for quality, an engine suited to
the model and hardware, and tuned settings. Choose a preset for the work you
want to do and the resources you can spare.

[Install Temper](https://github.com/temper-sh/temper#get-started), then run
`temper init` for a new setup or `temper configure` to change one. The wizard
shows the requirements and estimated memory use before you prepare anything.
[Follow the setup guide](https://github.com/temper-sh/temper/blob/master/docs/contracts/init.md) to get a model
running in your app.

This page describes the presets in the catalog dated 19 October 2026, for Apple
Silicon Macs. Profiles carrying the [memory observations below](#memory-observations)
need Temper 0.4.0 or newer. The other presets need 0.2.0, except Ettin Reranker,
which also works with 0.1.0.
Run `brew upgrade temper` to get the current release.

The catalog also lists two experimental engines, Rapid MLX and vLLM Metal,
with no presets yet. Temper 0.3.0 can use them for
[your own presets](https://github.com/temper-sh/temper/blob/master/docs/contracts/user-presets.md);
nothing here assesses them.

Chat presets let responses use the context remaining after the input, including
reasoning. They impose no separate output-token cap. Your app can request a
smaller allowance; a normal completion can end before either limit. The context
settings and memory allowances still apply.
Measurements retain their stated request budgets; an output allowance alone
does not establish completed-work quality.

The Gemma presets default to 1,024 thinking tokens before the answer. This
controls reasoning separately from the total response allowance. Apps can
override it with `reasoning_budget_tokens` (`-1` for unrestricted thinking,
`0` to end thinking immediately, or a positive token count).

## General recommendations

| Preset | Model license | Useful for | Main tradeoff |
|---|---|---|---|
| Qwen3.8 27B with Splash | [Apache-2.0][apache-license] | Coding and proposing changes to a repository | Faster first patch submissions in the tested comparison; patches still needed review. Requires Apple M3 or newer and macOS 26.4+. |
| Muse Glimmer 30B with DFlash2 | [Apache-2.0][apache-license] | Technical explanations, drafts, planning, and a second review | Faster generation with an extra draft model; source facts and operational constraints still need checking. |
| Gemma 4 26B-A4B | [Apache-2.0][apache-license] | Creative drafting, dialogue, and general chat | Shorter waits than 31B on the tested M5; facts and narrative continuity still need checking. |
| Gemma 4 31B | [Apache-2.0][apache-license] | Creative drafting and revision | Much longer waits on the tested M5; the small writing comparison did not establish a human preference advantage over 26B. |
| Qwen3.8 27B with llama.cpp | [Apache-2.0][apache-license] | Coding, combining sources, and document work | An alternative engine for Qwen; long inputs can be slow, and strict formats still need checking. |

The Qwen choices use the same target weights with different engines and
settings. Installing both reuses shared model files. Splash's first startup
also creates a weight cache, which adds time and disk use. The Splash 1.3.2
preset uses a 112,000-token total window under a 24 GiB engine memory cap;
118,000 was refused by this release on the tested M5 / 32 GiB. ANE prefill
calibrates during startup; SSD prefix caching is disabled. Short chat, JSON,
tool and prefix-reuse checks passed at the selected window, with 19.46 GiB
loaded and 20.46 GiB peak engine physical footprint, and no swap growth.
This checks startup and short requests at that allocation, not full-window
quality or a minimum machine RAM requirement. Prefix reuse took 0.36 s after
a 2.60 s cold request. The [Qwen assessment](../models/qwen3.8-27b/README.md)
retains coding measurements from its earlier engine.

Muse's DFlash2 preset uses llama.cpp b11741, a seven-token draft, medium thinking,
F16 conversation memory and a 131,072-token window. On M5 / 32 GiB it passed
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
and limits. **Other** retains the plain 57,344-token preset to save memory;
selecting both reuses the target weights. Saved plain selections stay plain.

### Gemma writing presets

Gemma 26B uses plain decoding with a 131,072-token window. Gemma 31B uses a
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
the matched populated-context check, so the default uses width 3.

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
(18.95 → 19.13 GiB). The catalog uses the larger capacity-workload peaks for
memory accounting. All these tests used llama-swap v265 and macOS 26.6.1,
one resident model and two-second memory sampling. They do not isolate an
engine-upgrade effect or establish vision, tool-use or multilingual quality.

These are recommendations for particular uses, not a ranking of every model.
We combine trusted upstream and practitioner findings with our own experiments,
compatibility checks, and measurements. The
[model assessments](../models/README.md) and [guides](../guides/README.md)
summarize the assessments and their limits.

## Memory observations

Temper uses an applicable catalog measurement before estimating memory. These
observations match the listed preset settings on Apple M5 / 32 GiB, macOS
26.6.1, with a 24 GiB Metal budget. Changing the engine, weights, runtime settings
or machine makes Temper use estimation until a matching measurement is available.

| Preset and tested engine | Date | Loaded → peak | Measured workload |
|---|---|---|---|
| Qwen/Splash 1.3.2, 112,000-token window | 2026-10-17 | 19.46 → 20.46 GiB engine physical footprint | Six short interface/reuse requests, largest input 522 tokens. Loaded after the first response. |
| Muse plain, llama.cpp b11741, 57,344-token window | 2026-10-19 | 15.79 → 16.13 GiB engine RSS | 49,152-token ledger retrieval, appended lookup, source correction, 512 generated tokens and follow-up. |
| Muse DFlash2, llama.cpp b11741, 131,072-token window | 2026-10-20 | 18.38 → 19.62 GiB engine RSS | 122,880-token ledger retrieval, appended lookup, source correction, 4,096 generated tokens and follow-up. |
| Gemma 26B plain, llama.cpp b11756, 131,072-token window | 2026-10-20 | 16.37 → 17.35 GiB engine RSS | Thinking-off capacity sequence: 122,880-input ledger, appended lookup, correction, 4,096 generated tokens and 126,969-input final recall. Loaded after readiness. |
| Gemma 31B MTP-3, llama.cpp b11756, 40,960-token window | 2026-10-20 | 18.95 → 19.97 GiB engine RSS | Thinking-off capacity sequence: 32,768-input ledger, appended lookup, correction, 4,096 generated tokens and 37,048-input final recall. Loaded after readiness. |
| TranslateGemma 4B, llama.cpp b11716, 4,096-token window | 2026-10-17 | Not sampled → 3.01 GiB engine RSS | Two short French/German translations, each with 84 input and 14 output tokens. |

These are observed peaks, not worst-case bounds or minimum machine RAM. The
engine counters exclude the router and, for Splash, its frontend. Splash RSS
and the tested llama.cpp physical-footprint counter omit much of the mapped
model memory, so the table uses different counters and does not add them to
overlapping system wired memory. No swap growth was observed. Short requests
do not establish full-window use; the larger Muse and Gemma workloads test capacity and
reuse, not general long-document accuracy. Other presets currently use estimates:
older-engine measurements and unmeasured configurations are not interchangeable.

## Specialist recommendations

Use **Specialist** to choose a model for one stage of a pipeline. Each task has
its own guidance; layouts let you keep small helpers available or load a
larger specialist on demand.

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
| Tiny Aya Fire | [CC BY-NC 4.0 · Noncommercial][noncommercial-license] | South Asian languages, including Hindi, Bengali, Gujarati, Marathi, Tamil, Telugu and Nepali | Official Q8_0, llama.cpp/Metal; 3.57 GB of weights. Regional recommendation follows Cohere; local language quality and comparable performance are unmeasured. |
| Tiny Aya Earth | [CC BY-NC 4.0 · Noncommercial][noncommercial-license] | West Asian and African languages, including Arabic, Persian, Turkish, Hebrew, Swahili, Amharic, Hausa and Yoruba | Official Q8_0, llama.cpp/Metal; 3.57 GB of weights. Regional recommendation follows Cohere. One Swahili check changed Tuesday to Monday; wider quality and comparable performance are unmeasured. |

Ettin's native SentenceTransformers route was faster in the same comparison
(0.39 s median) but used about 1.02 GiB RSS. The selected CrispEmbed conversion
retained retrieval quality on that 50-query sample with a smaller footprint.
It has a fixed 7,999-token query/passage limit. Split longer documents before
reranking. Qwen is the alternative when English-only retrieval is insufficient.

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
provide the language list and regional guidance. All four are recommended for
personal, noncommercial use with the license visible. They take a normal chat
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

## Other presets

The **Other** tab holds presets outside the curated recommendations: these
smaller models, and any saved preset that the catalog no longer recommends.
A smaller model can be your main assistant or a helper alongside another model.

| Preset | Model license | Useful for | Main limitation observed |
|---|---|---|---|
| Muse Glimmer 30B without draft | [Apache-2.0][apache-license] | The lower-memory Muse option for technical writing, planning and review | Slower generation than DFlash2 on the tested M5; the same source-fidelity and retrieval limitations still apply. |
| Gemma 4 E2B | [Apache-2.0][apache-license] | Extracting fields from short records and simple tool tasks | Some writing changed facts; recovery from a conflicting edit stopped before saving. |
| Gemma 4 E4B | [Apache-2.0][apache-license] | Short drafts from supplied facts and simple tool workflows | Longer editing retained incorrect claims after feedback. |
| Qwen3.5 4B | [Apache-2.0][apache-license] | Extraction, classification, and short tool workflows | Longer writing, translation, and editing need close checking. |

## Check what fits

Most local measurements come from an Apple M5 with 32 GiB. Fit on actual
8 or 16 GB Macs remains unverified. Memory tiers in the chooser are estimates,
and a model's file size is only part of its memory use.

Longer conversations, larger documents, and several loaded models need more
memory. Start with the preset's context setting and review the layout's
startup estimate. Selecting several alternatives does not require loading them
together.

The **Compute** control appears for NuExtract3 (GPU by default) and Qwen3
Reranker (CPU by default). CPU keeps the GPU available for other models;
GPU is faster, especially for NuExtract3 images. The selected device appears
before Context on the same settings line. Ettin shows **CPU only**; general
models and translators omit the device label and Compute control.
These are curated device choices, not every device an engine can run on.
CPU execution still needs RAM. Review shows the chosen device and recalculates
the memory estimate for your layout. Measurements apply to the named engine
and device.

General assistants default to medium thinking. Extractors and translators use
their direct-output interfaces; rerankers return scores. Some retained measurements used
thinking off or different software; they describe those tested configurations
and do not establish performance for every current setting.

## Keep or change a choice

Your saved presets stay fixed when this catalog changes. Nothing updates in
the background. Run `temper update` to get the newest catalog: it shows, for
each saved preset, what would change (old → new) and keeps your context and
compute choices. `temper update --software` changes only the engine and router
releases. Use `temper configure` to add or remove presets. Review any new
downloads, then activate the layout when you are ready.

- [Set up and manage layouts](https://github.com/temper-sh/temper/blob/master/docs/contracts/init.md)
- [Update or inspect the catalog](https://github.com/temper-sh/temper/blob/master/docs/CATALOG.md)
- [Author a preset or compile exact inputs](https://github.com/temper-sh/temper/blob/master/docs/contracts/execution-lock.md)

[apache-license]: https://choosealicense.com/licenses/apache-2.0/
[gemma-license]: https://ai.google.dev/gemma/terms
[noncommercial-license]: https://creativecommons.org/licenses/by-nc/4.0/
