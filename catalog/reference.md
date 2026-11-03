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
| Qwen / Splash | Unsloth K_XL, 112,000 tokens for 32 GiB+ and 131,072 for 64 GiB+ | Prepared BaseQuant Medium only, at 114,681; Unsloth retired from authoring |
| Qwen / Kapitsa | Not offered; parked experimental layout | MLX4/DFlash2 coding failures, successful cases and context measurements remain in the [reference](../models/qwen3.8-27b/layouts/kapitsa-mlx4-reference.md) |
| Muse Glimmer | DFlash2 at 131,072 tokens, or plain decoding at 57,344 | Matched plain/DFlash2 comparison at 57,344 tokens |
| Gemma 26B | Plain decoding, 131,072-token window and a 1,024-token reasoning budget | Earlier 98,304-token configuration and draft experiments |
| Gemma 31B | MTP width 3, 40,960-token window and a 1,024-token reasoning budget | Plain decoding, other MTP widths and draft methods |

The measurements below describe the named executions. Profiles containing
memory observations require Temper 0.4.0 or newer. Temper checks
each preset's compatibility before offering it; the signed catalog owns those
requirements.

## Thinking defaults and operator controls

Thinking is a request preference. Presets supply a fallback; supported explicit
harness settings take precedence. Display names describe the model and runtime.
The authoring source declares reasoning capability independently of its default.
Previously published IDs ending in `-off` remain stable lookup keys for saved
selections and forks; the suffix does not constrain requests. New preset IDs
use runtime names. Signed catalogs and existing locks change only through their
normal publication/update path.

| Reviewed presets | Capability and prepared fallback | Guidance and template finding |
|---|---|---|
| Qwen3.8 27B, BaseQuant Splash (1) | Switchable, on | [Publisher](https://huggingface.co/Qwen/Qwen3.8-27B) defaults on. The selected [Froggeric template](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates/blob/855bffc49448e299789730ff92c9b8d834d6cc14/chat_template.jinja) accepts `enable_thinking` and effort; the reviewed medium preset default is retained. Retired Unsloth Splash/MTP observations remain separate. |
| Qwen3.5 4B (1) | Switchable, on | [Publisher guidance](https://huggingface.co/Qwen/Qwen3.5-4B#best-practices) defaults on. The consumed GGUF template defaults off unless enabled. The prepared fallback explicitly enables it, with general-task thinking sampling T1 / top_p .95 / top_k 20 / presence penalty 1.5. |
| Gemma 4 E2B (1) | Switchable, off | [Google's card](https://huggingface.co/google/gemma-4-E2B-it) documents both modes and demonstrates off. The tested helper default is retained; no publisher requirement to enable thinking was found. |
| Gemma 4 E4B, 26B-A4B and 31B (3) | Switchable, on | [Google's card](https://huggingface.co/google/gemma-4-E4B-it) documents the switch. Consumed templates default off; explicit on preserves the assessed choices. The 512/1,024-token budgets remain overridable defaults grounded in the named local workloads, not universal model limits. |
| Nemotron 3.5 Bolt E4B, plain and DFlash (2) | Switchable, on | [NVIDIA's card](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Bolt-E4B-BF16#reasoning-on) defaults on; the consumed GGUF template defaults off. Both prepared presets explicitly enable it and retain their tested, overridable 512-token budgets. |
| Spark X2.6 4B (1) | Switchable, on | [Publisher](https://huggingface.co/XHToken/Spark-X2.6-4B) and consumed template default on. High is the template's effort default; only low/high are supported. Earlier off-mode helper measurements do not measure this fallback. |
| Muse Glimmer 30B, plain and DFlash2 (2) | Always thinking; template strength high | [Meta](https://huggingface.co/meta-models/Muse-Glimmer-30B) recommends high/xhigh for complex, coding and agentic work. The [selected template](https://huggingface.co/meta-models/Muse-Glimmer-30B/blob/cec94fcfd6512f2cfe708138c32533b346ea4f69/chat_template.jinja) reads `reasoning_strength`, defaults high, and has no thinking-off branch. The ineffective `reasoning_effort: medium` override is omitted. Use `chat_template_kwargs.reasoning_strength` or an explicit system directive to choose strength. |
| NuExtract3 (1) | Switchable, off | [NuMind](https://huggingface.co/numind/NuExtract3#reasoning-and-non-reasoning-modes) recommends off for production extraction and on for difficult examples. Its consumed template permits thinking only in structured/content modes. Existing measured sampling remains unchanged. |
| TranslateGemma 4B, 12B and 27B (3) | No thinking switch, off | Consumed translation templates have no reasoning control. [Model details](https://huggingface.co/bullerwins/translategemma-4b-it-GGUF) describe the translation-specific interface. |
| Tiny Aya Global, Water, Fire and Earth (4) | No thinking switch, off | The [official GGUF family](https://huggingface.co/CohereLabs/tiny-aya-global-GGUF) uses conversational templates without a thinking control. Retain the measured translation baseline. |
| Ettin 150M and Qwen3 Reranker 0.6B (2) | Not applicable | Native reranking interfaces have no generation defaults; a chat template in the Qwen weight file does not turn its scoring interface into a reasoning chat model. |

All 22 maintained presets were covered. Effort placeholders are omitted where
the consumed template does not read them; supported Qwen3.8, Spark and Splash
defaults remain explicit. Splash's unsupported neutral penalty placeholders are
also omitted. Resource limits, context profiles and tested reasoning budgets
remain as recorded; making a field optional does not erase a justified default.

Pi/PiG exports use the template's thinking switch for llama.cpp, so selecting
thinking in the harness can enable it even when the preset defaults off. Effort
levels in that switch select on; they do not promise matching effort levels in
every model. Muse's fixed behavior is a limitation, not a switchable mode.

Historical quality, latency and memory observations keep their tested mode and
budgets. Qwen3.5 and Spark's prepared on defaults have no new matched timing or
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


## Small collaborator configurations

These are prepared authoring choices, awaiting signed publication; the
[signed channel](v1/channels/stable/channel.yaml) remains the installation
authority. All use llama.cpp **b11869**, llama-swap **v265**, the embedded
GGUF template, one GPU slot, four threads, mmap, batch/microbatch 512 and Q8
KV. Prompt caching is enabled, RAM cache reserve is zero, and speculation is
disabled except for the named Bolt variant. New software is confined to these
choices; unrelated presets keep their existing engines.

| Prepared ID | Target and context | Request defaults | Context checkpoints |
|---|---|---|---:|
| `gemma-4-e2b-q4km-off` | Unsloth Q4_K_M, 32,768 | Off; T1 / top_p .95 / top_k 0 | 0 |
| `qwen3.5-4b-q4km-off` | Unsloth Q4_K_M, 16,384 | On; T1 / top_p .95 / top_k 20 / presence penalty 1.5 | 1 |
| `spark-x2.6-4b-q4km-llama` | Official Q4_K_M, 16,384 | On; T1 / top_p .95 / top_k 0 | 0 |
| `gemma-4-e4b-q4km-llama` | Unsloth Q4_K_M, 16,384 | On, reasoning 512; T1 / top_p .95 / top_k 0 | 0 |
| `nemotron-bolt-e4b-q4xl-llama` | Unsloth UD-Q4_K_XL, 16,384 | On, reasoning 512; T1 / top_p .95 / top_k 0 | 0 |
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
