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
| Qwen / Splash | BaseQuant Medium, 114,681 tokens | Retired Unsloth K_XL: historical 112,000-token 32 GiB and 131,072-token 64 GiB profiles, plus the matched 114,681-token comparison |
| Qwen / llama.cpp | Unsloth UD-Q4_K_XL / MTP, 65,536 tokens | [Document and capacity observations](../models/qwen3.8-27b/layouts/llama-cpp-reference.md) retain their earlier engines and workloads; M1/M2 fit is unmeasured |
| Qwen / Kapitsa | Not offered; parked experimental layout | MLX4/DFlash2 coding failures, successful cases and context measurements remain in the [reference](../models/qwen3.8-27b/layouts/kapitsa-mlx4-reference.md) |
| Muse Glimmer | DFlash2 at 131,072 tokens, or plain decoding at 57,344 | Matched plain/DFlash2 comparison at 57,344 tokens |
| Gemma 26B | Plain decoding, 131,072-token window and a 1,024-token reasoning budget | Earlier 98,304-token configuration and draft experiments |
| Gemma 31B | MTP width 3, 40,960-token window and a 1,024-token reasoning budget | Plain decoding, other MTP widths and draft methods |

The measurements below describe the named executions. Profiles containing
memory observations require Temper 0.4.0 or newer. Temper checks
each preset's compatibility before offering it; the signed catalog owns those
requirements.

## Sampling and context sources

The [authoring source](sources/apple-silicon.json) uses model-specific guidance
selected for the preset's designated role and reasoning mode, unless a documented
local comparison or workload justifies a different setting.
A successful short request is not evidence for choosing its test window as a
general default. Published snapshots and historical measurements retain their
original settings until an explicit catalog publication and user update.

`context_window_tokens` allocates input **plus output**. `context_limit_tokens`
is the preset's editable ceiling; it can be narrower than the architecture.
Neither number establishes useful long-context quality or fit on a particular
Mac. The table covers all 22 authoring presets. Larger fallback windows have no
new loaded/peak memory or latency measurements; review Temper's estimate and
adjust the allocation for your workload and machine.

| Preset(s) and designated role | Sampling basis | Allocation / catalog ceiling, tokens; reason |
|---|---|---|
| Qwen3.5 4B — application helper | [Qwen's general-task thinking recipe][settings-qwen35]: T1, top_p .95, top_k 20, presence penalty 1.5 | **262,144 / 262,144**, publisher default. Earlier 16,384 off-mode helper/cache measurements do not ground a thinking-on default; Qwen advises at least 128K for thinking. |
| Qwen3.8 BaseQuant Medium / Splash — coding and technical work | [Qwen's thinking recipe][settings-qwen38]: T1, top_p .95, top_k 20, presence penalty 0 | **114,681 / 262,144**. Retains the [admitted comparison allocation](../models/qwen3.8-27b/layouts/splash-ud-q4-k-xl-reference.md#basequant-medium-and-unsloth-k_xl) under the selected 24 GiB engine allowance. Actual occupied maximum was 41,769; this is not full-window proof. |
| Qwen3.8 Unsloth / llama.cpp — coding and general use | [Qwen's thinking recipe][settings-qwen38]: T1, top_p .95, top_k 20, presence penalty 0 | **65,536 / 262,144**. Retains the established allocation for the M1/M2 alternative; not a new capacity finding. [Earlier capacity observations](../models/qwen3.8-27b/layouts/llama-cpp-reference.md#long-documents-with-llamacpp) show why configured context is not proof of sustained fit. Current-engine loaded/peak memory and M1/M2 fit remain unmeasured. |
| Muse plain / DFlash2 — technical writing, planning and review | [Meta's generation configuration][settings-muse]: T1, top_p .95, top_k 64 | **57,344 / 131,072** plain; **131,072 / 131,072** DFlash2. Retains [populated retrieval, continuation and memory observations](#muse-glimmer-configurations), with a lower-memory plain alternative. |
| Gemma 4 E2B — note lookup and export helper | [Google's card][settings-e2b]: T1, top_p .95, top_k **64** | **32,768 / 131,072**. Retains the [selected 32K lookup/cache allocation](#small-collaborator-configurations), tested with 24.5K input. The former top_k 0 was a common test setting without a model-specific selection reason. |
| Gemma 4 E4B — booking and inventory helper | [Google's card][settings-e4b]: T1, top_p .95, top_k **64** | **16,384 / 131,072**. Retains the compact helper allocation demonstrated by [12.3K-input lookup and stateful tasks](../models/gemma-4-e4b/README.md); larger capacity and actual 16 GiB fit remain unmeasured. |
| Gemma 4 26B-A4B / 31B — creative drafting, conversation and revision | [26B][settings-gemma26] and [31B][settings-gemma31] cards: T1, top_p .95, top_k 64 | **131,072 / 262,144** and **40,960 / 262,144**, respectively. [Populated-context and writing tests](#gemma-writing-configurations) justify these machine-specific allocations, not a native model ceiling. |
| Nemotron Bolt plain / DFlash — stateful application helper | [NVIDIA's card][settings-bolt] supplies T1/top_p .95; its [generation configuration][settings-bolt-generation] supplies top_k **64** | Both **16,384 / 65,536**. Retains the [compact helper choice and populated lookup evidence](../models/nemotron-3.5-bolt-e4b/README.md). The earlier common top_k 0 has no demonstrated advantage. |
| Spark X2.6 4B — English application helper | [Publisher recipe][settings-spark]: T1, top_p .95, top_k disabled | **1,048,576 / 1,048,576**, publisher deployment default. The earlier 16K off-mode conversations used at most 869 input tokens; they do not justify restricting the on-mode window. The publisher explicitly permits reducing context for available memory. |
| NuExtract3 — structured extraction and document conversion | Greedy decoding retained: [nine local extraction cases](../models/specialists/nuextract3/README.md) and the publisher's [Transformers example][settings-nuextract] use it. The same card recommends T.2 for non-thinking sampling; greedy is a documented local choice. | **131,072 / 262,144**. Allocation follows the publisher's deployment recipe; [model configuration][settings-nuextract-config] supplies the architectural ceiling. The earlier 8,192 allocation only tested short documents. This does not transfer vLLM's memory behavior to llama.cpp. |
| Qwen3 Reranker 0.6B — multilingual relevance scoring | No generative sampler; native relevance scoring | **32,768 / 32,768**, [publisher's 32K bound][settings-reranker]. The 40,960 positional embedding size is not adopted over the card's narrower claim. The earlier 4,096 was a short-passage test allocation. |
| Ettin Reranker 150M — English relevance scoring | No generative sampler; native relevance scoring | **7,999 / 7,999**, the [model's maximum sequence length][settings-ettin]. Kept unchanged; a pair includes query, passage and formatting. |
| TranslateGemma 4B / 12B / 27B — translation | Greedy decoding follows all three Google cards: [4B][settings-translate4], [12B][settings-translate12], [27B][settings-translate27] | All **4,096 / 4,096**, a deliberately bounded translation allocation: Google documents **2K input**; the remaining space accommodates translation output. This total is a local policy, not Google's stated input limit or the inherited 131,072 architectural maximum. No long-document claim or separate output cap is introduced. |
| Tiny Aya Global / Water / Fire / Earth — translation | **Greedy (T0)**, selecting [Cohere's reported generative-evaluation setting][settings-aya-evaluation] for translation, consistent with the [local greedy baseline](../models/specialists/tiny-aya/README.md). The cards' T.1/top_p .95 examples are general usage, not translation-specific recommendations. No sampler-quality advantage is claimed. | All **8,192 / 8,192**, following the [Global][settings-aya-global], [Water][settings-aya-water], [Fire][settings-aya-fire] and [Earth][settings-aya-earth] cards' 8K context conservatively as a shared input/output window. The cards mention 8K input and output; these are not added together. The GGUF's 500,000 positional capacity is not a supported task-length claim. |

Qwen3.8 is the coding choice here; its publisher recommends the shown thinking
recipe without a separate coding sampler. Qwen3.5's lower-temperature coding
recipe does not apply to that model or to Qwen3.5's designated helper role.
Gemma, Muse, Bolt and Spark use their model-specific general recommendation
because the reviewed sources give no separate sampler for their catalog role.
NuExtract retains the documented extraction exception. Rerankers have no
generation sampler. Explicit supported request values override these defaults.

Extra filters remain disabled unless sourced: min_p 0, repeat penalty 1 and
presence penalty 0, except Qwen's explicit presence penalty. These neutral
settings prevent engine defaults from adding an extra restriction; they are not
tuned model parameters. Tiny Aya, TranslateGemma and NuExtract use top_k 1 and
top_p 1 alongside temperature 0 to express greedy decoding. Top-k/top-p do not
add a sampling policy to greedy generation.

The small-helper reasoning budgets remain scoped to their recorded workloads;
the larger Gemmas' 1,024-token choice has a separate [writing comparison](#gemma-writing-configurations).
Changing sampling can change quality, output length, latency and memory. Earlier
top_k 0 observations stay attached to those settings and do not validate the
replacement sampler. Tiny Aya's greedy baseline still does not validate the
larger configured window. Qwen3.5 and Spark have no current-default XS fit claim.

<details>
<summary>Source provenance</summary>

Checked 2026-11-04 against the pinned card/configuration revisions linked below
and the catalog's exact cached GGUF metadata. No model weights were downloaded
and no inference was performed. TranslateGemma and Tiny Aya's gated Transformer
configuration files were unavailable; the public publisher cards and locally
cached, catalog-pinned GGUF headers were used instead. Architectural metadata
does not override a narrower publisher task-length statement. Role review also
uses Cohere Labs maintainer `alexrs`'s 2 March 2026 reply on generation parameters,
checked 2026-11-04. This is reported evaluation practice, not a universal
translation recommendation. Regional defaults apply that family practice to
the shared translation role; regional quality remains bounded by each card's
local evidence.

</details>

[settings-qwen35]: https://huggingface.co/Qwen/Qwen3.5-4B/blob/851bf6e806efd8d0a36b00ddf55e13ccb7b8cd0a/README.md
[settings-qwen38]: https://huggingface.co/Qwen/Qwen3.8-27B/blob/1d4bf0f2ff6012fd82039f2fa52739d0dd7c60c0/README.md
[settings-muse]: https://huggingface.co/meta-models/Muse-Glimmer-30B/blob/cec94fcfd6512f2cfe708138c32533b346ea4f69/generation_config.json
[settings-e2b]: https://huggingface.co/google/gemma-4-E2B-it/blob/4adf15748de9e1a0097b6274cee9c15836c41c37/README.md
[settings-e4b]: https://huggingface.co/google/gemma-4-E4B-it/blob/4ecc2ba63bcac23ba4a767fc87025b1450f84c9c/README.md
[settings-gemma26]: https://huggingface.co/google/gemma-4-26B-A4B-it/blob/4d7ae4984b7db7de8f8457170b3f1a419ee76d52/README.md
[settings-gemma31]: https://huggingface.co/google/gemma-4-31B-it/blob/842da3794eaa0b77d5f08bae87a17459d91ff475/README.md
[settings-bolt]: https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Bolt-E4B-BF16/blob/49843eed8db1f1c616c47af75cfd36e07aec0667/README.md
[settings-bolt-generation]: https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Bolt-E4B-BF16/blob/49843eed8db1f1c616c47af75cfd36e07aec0667/generation_config.json
[settings-spark]: https://huggingface.co/XHToken/Spark-X2.6-4B/blob/aa074aa851209a683aeaf5f99a8e25a642d94541/README.md
[settings-nuextract]: https://huggingface.co/numind/NuExtract3/blob/c99dc8f5641b866aa0192b6ea78f84bf9f3535f1/README.md
[settings-nuextract-config]: https://huggingface.co/numind/NuExtract3/blob/c99dc8f5641b866aa0192b6ea78f84bf9f3535f1/config.json
[settings-reranker]: https://huggingface.co/Qwen/Qwen3-Reranker-0.6B/blob/e61197ed45024b0ed8a2d74b80b4d909f1255473/README.md
[settings-ettin]: https://huggingface.co/cross-encoder/ettin-reranker-150m-v1/blob/025501c4e0f9bbeb4c5b198318e0089ff061cc14/README.md
[settings-translate4]: https://huggingface.co/google/translategemma-4b-it/blob/10042cb0e6e7fdce748996a71dc3dc432a4e0c89/README.md
[settings-translate12]: https://huggingface.co/google/translategemma-12b-it/blob/d1b225e1caa17f1ddc7e62065d8637d0923f34e2/README.md
[settings-translate27]: https://huggingface.co/google/translategemma-27b-it/blob/7d10f0b72f89a2d0f268cea30727d8b77c0d25c2/README.md
[settings-aya-evaluation]: https://huggingface.co/CohereLabs/tiny-aya-global/discussions/8
[settings-aya-global]: https://huggingface.co/CohereLabs/tiny-aya-global/blob/af89d219b53ed9b13b8a4645f9c8028973510324/README.md
[settings-aya-water]: https://huggingface.co/CohereLabs/tiny-aya-water/blob/41a99216d12b3ef4881df762934186e5e4d2bbc1/README.md
[settings-aya-fire]: https://huggingface.co/CohereLabs/tiny-aya-fire/blob/73500c80fcb6c8bea3e830c21d5d2920249a0e1a/README.md
[settings-aya-earth]: https://huggingface.co/CohereLabs/tiny-aya-earth/blob/0624fafbe200f930b89566fc4ec4da314ba48d89/README.md

## Thinking defaults and operator controls

Thinking is a request preference. Presets supply a fallback; supported explicit
harness settings take precedence. Display names describe the model and runtime.
The authoring source declares reasoning capability independently of its default.
Previously published IDs ending in `-off` remain stable lookup keys for saved
selections and forks; the suffix does not constrain requests. New preset IDs
use runtime names. Signed catalogs and existing locks change only through their
normal publication/update path.

| Reviewed presets | Capability and default fallback | Guidance and template finding |
|---|---|---|
| Qwen3.8 27B, BaseQuant Splash (1) | Switchable, on | [Publisher](https://huggingface.co/Qwen/Qwen3.8-27B) defaults on. The selected [Froggeric template](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates/blob/855bffc49448e299789730ff92c9b8d834d6cc14/chat_template.jinja) accepts `enable_thinking` and effort; the reviewed medium preset default is retained. Retired Unsloth Splash/MTP observations remain separate. |
| Qwen3.5 4B (1) | Switchable, on | [Publisher guidance](https://huggingface.co/Qwen/Qwen3.5-4B#best-practices) defaults on. The consumed GGUF template defaults off unless enabled. The fallback explicitly enables it, with general-task thinking sampling T1 / top_p .95 / top_k 20 / presence penalty 1.5. |
| Gemma 4 E2B (1) | Switchable, off | [Google's card](https://huggingface.co/google/gemma-4-E2B-it) documents both modes and demonstrates off. The tested helper default is retained; no publisher requirement to enable thinking was found. |
| Gemma 4 E4B, 26B-A4B and 31B (3) | Switchable, on | [Google's card](https://huggingface.co/google/gemma-4-E4B-it) documents the switch. Consumed templates default off; explicit on preserves the assessed choices. The 512/1,024-token budgets remain overridable defaults grounded in the named local workloads, not universal model limits. |
| Nemotron 3.5 Bolt E4B, plain and DFlash (2) | Switchable, on | [NVIDIA's card](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Bolt-E4B-BF16#reasoning-on) defaults on; the consumed GGUF template defaults off. Both presets explicitly enable it and retain their tested, overridable 512-token budgets. |
| Spark X2.6 4B (1) | Switchable, on | [Publisher](https://huggingface.co/XHToken/Spark-X2.6-4B) and consumed template default on. High is the template's effort default; only low/high are supported. Earlier off-mode helper measurements do not measure this fallback. |
| Muse Glimmer 30B, plain and DFlash2 (2) | Always thinking; template strength high | [Meta](https://huggingface.co/meta-models/Muse-Glimmer-30B) recommends high/xhigh for complex, coding and agentic work. The [selected template](https://huggingface.co/meta-models/Muse-Glimmer-30B/blob/cec94fcfd6512f2cfe708138c32533b346ea4f69/chat_template.jinja) reads `reasoning_strength`, defaults high, and has no thinking-off branch. The ineffective `reasoning_effort: medium` override is omitted. Use `chat_template_kwargs.reasoning_strength` or an explicit system directive to choose strength. |
| NuExtract3 (1) | Switchable, off | [NuMind](https://huggingface.co/numind/NuExtract3#reasoning-and-non-reasoning-modes) recommends off for production extraction and on for difficult examples. Its consumed template permits thinking only in structured/content modes. Existing measured sampling remains unchanged. |
| TranslateGemma 4B, 12B and 27B (3) | No thinking switch, off | Consumed translation templates have no reasoning control. [Model details](https://huggingface.co/bullerwins/translategemma-4b-it-GGUF) describe the translation-specific interface. |
| Tiny Aya Global, Water, Fire and Earth (4) | No thinking switch, off | The [official GGUF family](https://huggingface.co/CohereLabs/tiny-aya-global-GGUF) uses conversational templates without a thinking control. Greedy sampling follows the selected generative-evaluation practice for translation; context follows the cards. Historical tests retain their smaller allocation. |
| Ettin 150M and Qwen3 Reranker 0.6B (2) | Not applicable | Native reranking interfaces have no generation defaults; a chat template in the Qwen weight file does not turn its scoring interface into a reasoning chat model. |

All 21 maintained presets are covered. Effort placeholders are omitted where
the consumed template does not read them; supported Qwen3.8, Spark and Splash
defaults remain explicit. Splash's unsupported neutral penalty placeholders are
also omitted. The [sampling and context source table](#sampling-and-context-sources)
owns current allocations and sampler rationale; making a field optional does not
erase a justified default.

Pi/PiG exports use the template's thinking switch for llama.cpp, so selecting
thinking in the harness can enable it even when the preset defaults off. Effort
levels in that switch select on; they do not promise matching effort levels in
every model. Muse's fixed behavior is a limitation, not a switchable mode.

Historical quality, latency and memory observations keep their tested mode and
budgets. Qwen3.5 and Spark's on defaults have no new matched timing or
loaded/peak memory measurements. Their off-mode engine RSS measurements below
remain workload observations, not forecasts for thinking-on use. Removing an
engine setting can also make a saved memory observation inapplicable; Temper
then estimates instead. Earlier Muse records saying “medium effort” do not by
themselves establish effective template strength; an actual system directive or
`reasoning_strength` value is needed to substantiate that comparison.

<details>
<summary>Review provenance</summary>

Checked 2026-10-30 against publisher cards, the exact GGUF metadata and external
template revisions referenced by the authoring source. Only file metadata and
templates were read; no inference or native model runs were performed. The
pinned llama.cpp [request implementation](https://github.com/ggml-org/llama.cpp/blob/b11741/tools/server/server-common.cpp)
merges request template kwargs over launch defaults. A positive OpenAI-style
`reasoning_effort` alone does not enable thinking over an off default; use
`chat_template_kwargs.enable_thinking`. It does not translate `reasoning_effort`
to Muse's `reasoning_strength`. The
[Splash frontend](https://github.com/incoai/splash/blob/be3b083c5f413673d44e44881042e875e674e333/server/server.py)
permits an absent request timeout; existing sourced timeout defaults are retained.
The release/update process remains responsible for native integration validation.

</details>

## Response and reasoning settings

Chat presets let responses use the context remaining after the input, including
reasoning. They impose no separate output-token cap. Your app can request a
smaller allowance; a normal completion can end before either limit. The context
settings and memory allowances still apply.
Measurements retain their stated request budgets; an output allowance alone
does not establish completed-work quality.

The evaluated larger Gemma configurations below allow up to 1,024 thinking tokens
before the answer. This controls reasoning separately from the total response allowance. Apps can
override it with `reasoning_budget_tokens` (`-1` for unrestricted thinking,
`0` to end thinking immediately, or a positive token count).

## Qwen / Splash

The [Splash technical reference](../models/qwen3.8-27b/layouts/splash-ud-q4-k-xl-reference.md)
owns the configuration comparison, coding review findings and workload details.
The memory table below retains the observations referenced by catalog records.

## Muse Glimmer configurations

The evaluated Muse DFlash2 configuration uses llama.cpp b11741, a seven-token
draft, requested medium effort, F16 conversation memory and a 131,072-token window.
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

The reasoning budget has a specific comparison behind it: the first 26B scene
used all 4,096 response tokens thinking, then completed with an 8,192 allowance
in 4,699 tokens / 111.47 s. A 1,024-token reasoning budget produced a correct
scene in 1,362 total tokens / 32.09 s; 512 was faster but changed required facts.
The other frozen writing tasks confirmed the selected 1,024 point for both
sizes. Per-attempt loaded/peak memory for this budget comparison is not available
in the retained summary; the five-task measurements above describe their own
workloads. This is evidence for a writing default, not a universal reasoning limit.


## Small collaborator configurations

These presets are offered through the
[signed channel](v1/channels/stable/channel.yaml), the installation authority. The helper measurements used llama.cpp **b11869**, llama-swap **v265**, the embedded
GGUF template, one GPU slot, four threads, mmap, batch/microbatch 512 and Q8
KV. Prompt caching is enabled, RAM cache reserve is zero, and speculation is
disabled except for the named Bolt variant. Current authoring uses b12089 and
v269. The table gives current defaults; the [source table](#sampling-and-context-sources)
explains changes from the measured samplers and windows. Those changes have no
new quality, latency or memory measurements.

| Preset ID | Target and context | Request defaults | Context checkpoints |
|---|---|---|---:|
| `gemma-4-e2b-q4km-off` | Unsloth Q4_K_M, 32,768 | Off; T1 / top_p .95 / top_k 64 | 0 |
| `qwen3.5-4b-q4km-off` | Unsloth Q4_K_M, 262,144 | On; T1 / top_p .95 / top_k 20 / presence penalty 1.5 | 1 |
| `spark-x2.6-4b-q4km-llama` | Official Q4_K_M, 1,048,576 | On; T1 / top_p .95 / top_k 0 | 0 |
| `gemma-4-e4b-q4km-llama` | Unsloth Q4_K_M, 16,384 | On, reasoning 512; T1 / top_p .95 / top_k 64 | 0 |
| `nemotron-bolt-e4b-q4xl-llama` | Unsloth UD-Q4_K_XL, 16,384 | On, reasoning 512; T1 / top_p .95 / top_k 64 | 0 |
| `nemotron-bolt-e4b-q4xl-dflash-llama` | Same Bolt target plus independent NVIDIA BF16 draft, width 7, 16,384 | Same Bolt reasoning/sampling | 0 |

All use min_p 0 and repeat penalty 1; presence penalty is zero except Qwen.
No separate total-output cap is selected. The historical conversation confirmation allowed
512 tokens per request off or 2,048 on, with the separate 512-token reasoning
budget. The ordinary lookup probe inherits the selected policy. E2B's larger
probe reserves 4,096 output tokens but generates only 57–67, so it does not
prove long generation. Actual inputs/outputs and memory are retained below
and on the owning model cards.

E2B advances one useful context step after correct 16K lookups: all three
24.5K-input requests at 32K return correct values, reuse the appended prefix
and reprocess an early correction. JSON fences remain a strict-format defect.
Short read/export/cancel work remains useful at the larger allocation. No 64K
ladder or context search across every model was run; 32K is not a maximum.

Qwen advances one known cache setting: one checkpoint reproduces the 13 tested
booking/read/export/cancel phases and cuts the populated append from 19.32 to
2.00 s. RSS for that probe changes from 3.04→3.11 to 3.04→3.16 GiB; wired
growth is respectively 3.06→3.14 and 3.05→3.12 GiB. It retains exactly the
same wrong lookup values. This selects reuse, not a retrieval-quality fix.

Bolt's matched DFlash treatment retains the named stateful outcomes and new
false-ownership refusal while reducing median turn time from 11.35 to 5.84 s.
Conversation RSS is 5.07→5.15 GiB plain versus 5.86→6.29 GiB DFlash; wired
growth is 3.63→3.68 versus 5.36→5.39 GiB. The lower-memory plain choice stays
available in the proposal. Width 7 is the tested point, not an optimum. The
[model assessment](../models/nemotron-3.5-bolt-e4b/README.md) preserves the
invalid first comparison and the corrected request policy.

The one-seed deployment confirmation does not replace the older three-seed
helper evidence or qualify a general assistant. New false-ownership tests fail
for E2B, E4B, Qwen and Spark; both matched Bolt deployments pass this sample.
Permission enforcement still belongs in the application. Synthetic capability
breadth, completed work, cache behavior and real low-memory fit remain distinct.
The E4B runtime ID does not rename the historical off-mode entry or its
measurements; existing saved selections are not rewritten.

[FrogNano's native MTP results](../models/frognano-4b/README.md) remain separate:
the imported MLX/MTP composition is not expressible by Temper's public-MLX /
DFlash2 Kapitsa contract. Kapitsa is parked; no native FrogNano preset is
prepared. No scratch conversion is presented as a public native preset. Granite remains an assessed copyediting/lookup option, without adding
another action-helper choice after its known ownership failures.

## Memory observations

Temper 0.4.0 uses an applicable catalog measurement before estimating memory. These
observations describe their recorded settings on Apple M5 / 32 GiB, macOS
26.6.1, with a 24 GiB Metal budget, except the explicitly named M5 Pro / 64 GiB
row (macOS 26.7.1, 53,084 MiB Metal budget). Changing the engine, weights, runtime
settings or machine makes Temper use estimation until a matching measurement
is available.

| Preset and tested engine | Date | Loaded → peak | Measured workload |
|---|---|---|---|
| Qwen/BaseQuant Medium, Splash 1.3.2, 114,681-token window | 2026-11-01 | 18.27 → 21.11 GiB system wired-memory growth | Actual-history CSV review repair; maximum occupied 41,769 tokens. Passed 15 frozen checks, rejected after source review. |
| Qwen/Unsloth K_XL, Splash 1.3.2, 114,681-token window | 2026-11-01 | 18.89 → 21.70 GiB system wired-memory growth | Actual-history CSV review repair; maximum occupied 60,314 tokens. Passed 15 frozen checks, rejected after source review. |
| Qwen/Splash 1.3.2, 112,000-token window | 2026-10-17 | 19.46 → 20.46 GiB engine physical footprint | Six short interface/reuse requests, largest input 522 tokens. Loaded after the first response. |
| Qwen/Splash 1.3.2, M5 Pro / 64 GiB, 131,072-token window | 2026-10-20 | 19.22 → 21.63 GiB system wired-memory growth | Two-response coding workflow, thinking on; 16,369 input + 43,375 generated tokens in the first response, then 20,846 input + 4,018 generated. Loaded after readiness. Submitted code failed independent tests; resource measurements remain applicable. |
| Muse plain, llama.cpp b11741, 57,344-token window | 2026-10-19 | 15.79 → 16.13 GiB engine RSS | 49,152-token ledger retrieval, appended lookup, source correction, 512 generated tokens and follow-up. |
| Muse DFlash2, llama.cpp b11741, 131,072-token window | 2026-10-20 | 18.38 → 19.62 GiB engine RSS | 122,880-token ledger retrieval, appended lookup, source correction, 4,096 generated tokens and follow-up. |
| Gemma 26B plain, llama.cpp b11756, 131,072-token window | 2026-10-20 | 16.37 → 17.35 GiB engine RSS | Thinking-off capacity sequence: 122,880-input ledger, appended lookup, correction, 4,096 generated tokens and 126,969-input final recall. Loaded after readiness. |
| Gemma 31B MTP-3, llama.cpp b11756, 40,960-token window | 2026-10-20 | 18.95 → 19.97 GiB engine RSS | Thinking-off capacity sequence: 32,768-input ledger, appended lookup, correction, 4,096 generated tokens and 37,048-input final recall. Loaded after readiness. |
| E2B off, b11869, 32,768-token window | 2026-10-25 | 3.19 → 3.31 GiB engine RSS | 1,024-record cold/append/early-edit lookup, 24,496–24,606 input and 57–67 output tokens/request. Values correct, JSON fences. |
| Qwen3.5 4B off, b11869, 16,384-token window, one checkpoint | 2026-10-25 | 3.04 → 3.16 GiB engine RSS | 512-record lookup sequence, 12,261–12,357 input and 35–53 output tokens/request. Wrong values retained, real append reuse. |
| Spark X2.6 off, b11869, 16,384-token window | 2026-10-25 | 3.79 → 3.84 GiB engine RSS | Eight short application dialogues / 24 turns / 39 responses; 136–869 input tokens/request and 946 total output. Includes wrong work; populated context unmeasured. |
| E4B thinking 512, b11869, 16,384-token window | 2026-10-25 | 4.99 → 5.10 GiB engine RSS | 512-record lookup sequence, 12,279–12,385 input and 179–239 output tokens/request. Correct values, JSON fences. |
| Bolt thinking 512 plain, b11869, 16,384-token window | 2026-10-25 | 5.07 → 5.17 GiB engine RSS | 512-record lookup sequence, 12,279–12,371 input and 163–314 output tokens/request. Correct bare JSON. |
| Bolt thinking 512 DFlash 7, b11869, 16,384-token window | 2026-10-25 | 5.86 → 6.29 GiB engine RSS | Eight short dialogues / 24 turns / 43 responses; 133–841 input tokens/request and 6,682 total output. Populated probe separately passed at a lower peak. |
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
