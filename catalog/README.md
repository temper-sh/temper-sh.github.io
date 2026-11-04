# Explore presets

A preset combines model weights, an engine, settings and any template fixes or
adapters. Temper prepares that combination for your applications. The entries
below explain the available choices and link to observations; our limited tests
do not establish a general quality ranking.

**[Install Temper](https://github.com/temper-sh/temper#get-started)**, then use
`temper init` for a new setup or `temper configure` to change one. Review the
requirements and memory estimate before preparing the selected models.

The [sampling and context reference](reference.md#sampling-and-context-sources)
links the publisher guidance and local evidence behind each authoring default.
Model capacity, allocated context and demonstrated workload length are distinct;
publisher-sized windows can require substantially more memory than short tests.

## General assistants

<a id="general-recommendations"></a>

| Preset | Why this configuration exists | Observations |
|---|---|---|
| Qwen3.8 27B with Splash | Uses the Unsloth weight build with a draft model and the Frog template integration. Requires Apple M3+, macOS 26.4+ and 32 GiB+. | [Coding trials, hardware behavior and limitations](../models/qwen3.8-27b/layouts/splash-ud-q4-k-xl.md). |
| Qwen3.8 27B with llama.cpp | Runs the same target weights through another engine, with the Frog template and built-in draft prediction. | [Coding and document-work cases](../models/qwen3.8-27b/README.md). |
| Muse Glimmer 30B | DFlash2 accelerates generation on the tested M5 at a 131,072-token window; the plain 57,344-token option uses less memory. | [Technical writing and workflow observations](../models/muse-glimmer-30b/README.md). |
| Gemma 4 26B-A4B | Unsloth QAT weights, plain decoding and a 131,072-token window, with a 1,024-token reasoning default. | [Writing and conversation cases](../models/gemma-4-26b-a4b/README.md). |
| Gemma 4 31B | The larger Gemma model uses MTP width 3 at a 40,960-token window, with a 1,024-token reasoning default. | [Writing cases and measured costs](../models/gemma-4-31b/README.md). |

**Prepared Qwen choice:** BaseQuant Medium with Splash is the sole Qwen3.8 27B
preset in the authoring source, using the tested 114,681-token allocation.
Both Unsloth presets, Splash and llama.cpp/MTP, are retired from that source.
In the comparison's
CSV review repair, loaded/peak system wired growth was **18.27/21.11 GiB**
for Medium and **18.89/21.70 GiB** for K_XL. Both remained unaccepted after
repair; cumulative model-active costs were about 66 minutes each. These are
workload observations including other applications, not minimum RAM or a
quality ranking. [Conditions and limitations](../models/qwen3.8-27b/layouts/splash-ud-q4-k-xl-reference.md#basequant-medium-and-unsloth-k_xl)
retain the exact evidence. The signed catalog still supplies Unsloth at
112,000 on 32 GiB; `temper update` will not receive the prepared changes until
signed publication. Already saved Unsloth presets remain usable;
updating the catalog neither removes them nor switches their weights to BaseQuant.
Temper versions supporting the current retirement notice offer **Retired** in
`configure` for an explicit replacement or removal. Older versions and notices
that have aged out use **Other**.
Full `temper update` can still update their router/engine software while keeping
weights and settings. To change the model, select BaseQuant explicitly.

These models use the [Apache-2.0 license][apache-license]. The signed Unsloth
Splash and llama.cpp presets share their target file. Splash also creates a
native weight cache, adding disk use and first-startup work.

The [Qwen / Kapitsa layout](../models/qwen3.8-27b/layouts/kapitsa-mlx4.md) is
parked experimental and is not offered in the current catalog. Coding failures
prevent a recommendation; its measured outcomes remain available for review.
Existing saved presets remain under **Other** when the catalog is updated.

Profiles containing memory observations require Temper 0.4.0 or
newer. The linked observations include Splash's separate 64 GiB profile and
configurations beyond the published presets, such as alternative Gemma draft
settings. The
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

These configurations serve different small application jobs rather than one
universal assistant role. Actual fit on 8 or 16 GiB machines remains unmeasured.

**Availability:** the signed catalog contains the earlier Gemma E2B, Gemma E4B
and Qwen3.5 presets. The assessed configurations below are prepared in the
[authoring source](sources/apple-silicon.json), awaiting signed publication;
`temper update` continues to use the signed choices. Their memory observations
require Temper 0.4.0 or newer. Existing saved selections are unchanged.

| Prepared preset | Useful distinction | Assessment |
|---|---|---|
| Gemma 4 E2B, 32K | Fast note reads and export conversations; correct populated lookups with unwanted JSON fences | [Behavior and memory](../models/gemma-4-e2b/README.md). |
| Qwen3.5 4B, 256K | Publisher thinking-on window; one checkpoint for continuation reuse. Helper measurements used 16K/off; full-window fit is unmeasured. | [Ownership and lookup limits](../models/qwen3.5-4b/README.md). |
| Spark X2.6 4B, 1M | Publisher thinking-on window; fast helper observations used 16K/off. Full-window fit is unmeasured. | [Stock errors and reasoning limits](../models/spark-x2.6-4b/README.md). |
| Gemma 4 E4B, 16K | More deliberate booking and inventory actions; longer waits and false-ownership failures | [Observed work](../models/gemma-4-e4b/README.md). |
| Nemotron Bolt E4B, DFlash 7, 16K | Faster Bolt actions at a higher memory cost | [Matched speed/memory comparison](../models/nemotron-3.5-bolt-e4b/README.md). |
| Nemotron Bolt E4B, plain, 16K | The lower-memory Bolt alternative | [Actions, grounding and limitations](../models/nemotron-3.5-bolt-e4b/README.md). |

Thinking is a preset default that the harness can override where supported.
[Defaults, capabilities and publisher guidance](reference.md#thinking-defaults-and-operator-controls)
explain the exceptions and distinguish prepared settings from measured modes.

All use Q4 weights and Q8 conversation memory; these are different precision
choices. Gemma, Qwen and Spark use [Apache-2.0][apache-license]; Bolt and its
independent NVIDIA draft use [OpenMDW-1.1](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Bolt-E4B-BF16).
Temper shows these under **Other**, which is not a quality ranking. Applications
must enforce permissions even where the ownership sample succeeds.

[FrogNano's imported native MTP configuration](../models/frognano-4b/README.md)
is also a useful measured collaborator, but Temper cannot express that runtime
composition, and Kapitsa is parked. These are experimental measurements,
not an installable native preset.
[Exact configurations and memory records](reference.md#small-collaborator-configurations)
keep these choices separate from historical tests.

## Check what fits

Most measurements use an M5 with 32 GiB; some Qwen measurements use an M5 Pro
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
