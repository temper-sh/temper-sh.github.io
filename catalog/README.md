# Choose a preset

Temper pairs each model with weights chosen for quality, an engine suited to
the model and hardware, and tuned settings. Choose a preset for the work you
want to do and the resources you can spare.

[Install Temper](https://github.com/temper-sh/temper#get-started), then run
`temper init` for a new setup or `temper configure` to change one. The wizard
shows the requirements and estimated memory use before you prepare anything.
[Follow the setup guide](https://github.com/temper-sh/temper/blob/master/docs/contracts/init.md) to get a model
running in your app.

This page describes the presets in the catalog dated 16 October 2026, for Apple
Silicon Macs. They need Temper 0.2.0 or newer; Temper 0.1.0 can use only the
Ettin Reranker. Run `brew upgrade temper` to get the new release.

Chat presets let responses use the context remaining after the input, including
reasoning. They impose no separate output-token cap. Your app can request a
smaller allowance; a normal completion can end before either limit. The context
settings and memory allowances still apply.
Measurements below retain their original request budgets; they do not establish
long-output performance.

## General recommendations

| Preset | Model license | Useful for | Main tradeoff |
|---|---|---|---|
| Qwen3.8 27B with Splash | [Apache-2.0][apache-license] | Coding and proposing changes to a repository | Faster first patch submissions in the tested comparison; patches still needed review. Requires Apple M3 or newer and macOS 26.4+. |
| Muse Glimmer 30B | [Apache-2.0][apache-license] | Technical explanations, drafts, planning, and a second review | Useful structure and explanations; database and rollout advice needed substantive corrections. |
| Gemma 4 26B-A4B | [Apache-2.0][apache-license] | Creative drafting, dialogue, and general chat | Shorter waits than 31B on the tested M5; facts and narrative continuity still need checking. |
| Gemma 4 31B | [Apache-2.0][apache-license] | Creative drafting and revision | Much longer waits on the tested M5; the small writing comparison did not establish a human preference advantage over 26B. |
| Qwen3.8 27B with llama.cpp | [Apache-2.0][apache-license] | Coding, combining sources, and document work | An alternative engine for Qwen; long inputs can be slow, and strict formats still need checking. |

The Qwen choices use the same target weights with different engines and
settings. Installing both reuses shared model files. Splash's first startup
also creates a weight cache, which adds time and disk use.

These are recommendations for particular uses, not a ranking of every model.
We combine trusted upstream and practitioner findings with our own experiments,
compatibility checks, and measurements. The
[model assessments](../models/README.md) and [guides](../guides/README.md)
summarize the assessments and their limits.

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
F16 KV, and disable speculation. The selected
input adapter preserves the publisher's translation prompt. A six-case check
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
