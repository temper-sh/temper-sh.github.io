# Explore presets

A preset combines model weights, an engine, settings and any template fixes or
adapters. Temper prepares that combination for your applications. The entries
below explain the available choices and link to observations; our limited tests
do not establish a general quality ranking.

**[Install Temper](https://github.com/temper-sh/temper#get-started)**, then use
`temper init` for a new setup or `temper configure` to change one. Review the
requirements and memory estimate before preparing the selected models.

## General assistants

<a id="general-recommendations"></a>

| Preset | Why this configuration exists | Observations |
|---|---|---|
| Qwen3.8 27B with Splash | Uses the Unsloth weight build with a draft model and the Frog template integration. Requires Apple M3+, macOS 26.4+ and 32 GiB+. | [Coding trials, hardware behavior and limitations](../models/qwen3.8-27b/layouts/splash-ud-q4-k-xl.md). |
| Qwen3.8 27B with llama.cpp | Runs the same target weights through another engine, with the Frog template and built-in draft prediction. | [Coding and document-work cases](../models/qwen3.8-27b/README.md). |
| Muse Glimmer 30B | Uses plain decoding, keeping the setup free of a separate draft model. | [Technical writing and workflow observations](../models/muse-glimmer-30b/README.md). |
| Gemma 4 26B-A4B | Pairs Unsloth QAT weights with llama.cpp and the embedded template; the published preset uses plain decoding. | [Writing and conversation cases](../models/gemma-4-26b-a4b/README.md). |
| Gemma 4 31B | Offers the larger Gemma model with the same weight-build family; the published preset also uses plain decoding. | [Writing cases and measured costs](../models/gemma-4-31b/README.md). |

These models use the [Apache-2.0 license][apache-license]. Shared model files are
reused when selecting both Qwen presets. Splash also creates a native weight
cache, adding disk use and first-startup work.

The linked observations include configurations beyond the published presets.
The [32 GiB Qwen recommendation uses Kapitsa](../models/qwen3.8-27b/layouts/kapitsa-mlx4.md)
and requires direct setup with the published Temper version. The authored
catalog candidate `qwen3.8-27b-mlx4-kapitsa` requires Temper 0.4.0 and is not
available in the signed catalog yet. It uses native Kapitsa v0.2.9, MLX4 target
weights and DFlash2 at 98,304 tokens, with the configuration and memory evidence
in the [Kapitsa reference](../models/qwen3.8-27b/layouts/kapitsa-mlx4-reference.md).
Splash's larger 64 GiB profile, Muse DFlash2, and the larger-context or draft-assisted
Gemma variants are evaluated configurations. The
[configuration reference](reference.md#published-settings-and-evaluated-variants)
distinguishes them from the offerings in the signed catalog.

## Specialists

<a id="specialist-recommendations"></a>

These presets expose an interface for a defined job. See the
[specialist request guide](https://github.com/temper-sh/temper/blob/master/docs/SPECIALISTS.md)
for the inputs your application supplies.

| Preset | Interface and configuration rationale | License and observations |
|---|---|---|
| NuExtract3 4B | Text or document image → structured records; includes the image projector, with GPU and CPU execution choices. | [Apache-2.0][apache-license] · [Extraction observations](../models/specialists/nuextract3/README.md). |
| Ettin Reranker 150M | English query and passages → scores; the CrispEmbed CPU conversion keeps the component small. | [Apache-2.0][apache-license] · [Ranking and memory comparison](../models/specialists/ettin-reranker/README.md). |
| Qwen3 Reranker 0.6B | Multilingual query and passages → scores; llama.cpp provides CPU and GPU choices. | [Apache-2.0][apache-license] · [Ranking observations](../models/specialists/qwen3-reranker/README.md). |
| TranslateGemma 4B / 12B / 27B | Text and language codes → translation; the embedded prompt and native language-code handling preserve the publisher's interface. Sizes have different resource costs. | [Gemma terms][gemma-license] · [Translation observations](../models/specialists/translategemma/README.md). |
| Tiny Aya Global / Water / Fire / Earth | Chat instructions → translation; official Q8_0 builds retain the embedded template. Variants have different publisher-described regional coverage. | [Noncommercial][noncommercial-license] · [Language coverage and observations](../models/specialists/tiny-aya/README.md). |

Publisher language coverage is separate from local validation. The translation
samples found meaning errors; some language regions remain locally unmeasured.
The cards identify the actual cases and review method.

## Smaller assistants

<a id="other-presets"></a>

These presets use smaller model builds for lower resource budgets. They can be
main assistants or helpers, including on larger machines. Their measurements
do not establish fit on actual 8 or 16 GiB machines.

| Preset | Configuration | Recorded work |
|---|---|---|
| Gemma 4 E2B | llama.cpp, Q4_K_M, embedded template | [Field extraction and short tool cases](../models/gemma-4-e2b/README.md). |
| Gemma 4 E4B | llama.cpp, Q4_K_M, embedded template | [Short drafts and editing cases](../models/gemma-4-e4b/README.md). |
| Qwen3.5 4B | llama.cpp, Q4_K_M | [Extraction, classification and tool cases](../models/qwen3.5-4b/README.md). |

All three use [Apache-2.0][apache-license]. Temper may show these under **Other**;
that grouping is not a measured quality score.

## Check what fits

Most measurements use an M5 with 32 GiB; some Splash measurements use an M5 Pro
with 64 GiB. Conversation history, the engine and other applications also need
memory. Weight-file size and a measured process peak are not minimum machine RAM.

A layout can keep several presets available on demand. Review the estimate for
what you intend to load together. The
[setup guide](https://github.com/temper-sh/temper/blob/master/docs/contracts/init.md)
explains selection, device controls and activation.

## Memory observations

[Measured configurations, loaded/peak counters and workload limits](reference.md#memory-observations)
support the catalog's memory records. Read the named conditions before applying
a measurement to another setup.

## Keep or change a choice

Saved selections remain fixed until you choose to update them. See
[Temper's update documentation](https://github.com/temper-sh/temper/blob/master/docs/CATALOG.md)
for catalog and software updates.

[Models, tasks and hardware](../guides/choose-a-local-model.md) ·
[Technical references](../guides/technical-reference.md) ·
[Back to Temper](../README.md).

[apache-license]: https://choosealicense.com/licenses/apache-2.0/
[gemma-license]: https://ai.google.dev/gemma/terms
[noncommercial-license]: https://creativecommons.org/licenses/by-nc/4.0/
