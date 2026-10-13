# Specialist models for pipelines

Choose a model for a defined step: extract records, reorder search results or
translate text. The useful differences are the input/output contract, languages,
correctness and resource cost. Each linked card explains the evidence behind
its recommendation.

Choose a preset in [Temper's Specialist tab](../../catalog/README.md#specialist-recommendations).
Your application connects the steps; a layout makes the models available and
lets them load when requested. The [request guide](https://github.com/temper-sh/temper/blob/master/docs/SPECIALISTS.md)
shows their different APIs.

## Structured extraction

**Text or document image → records matching your schema.** Use an extractor
before storing facts, filling a form or handing a compact record to another
model. Check record associations and missing values as well as valid JSON.

**[NuExtract3 4B](nuextract3/README.md)** is the native extraction choice,
under [Apache-2.0][apache]. It takes a template and instructions, supports
images, and has local evidence in English and one Swedish case. Q4_K_M with
its BF16 projector used **3.69 GiB peak RSS on GPU** or **6.31 GiB on CPU**.
Prefer GPU for images: the tested invoice took 3 seconds versus 145 on CPU.

If [Qwen3.5 4B](../qwen3.5-4b/README.md#structured-extraction) is already in your
pipeline, try its JSON Schema interface before adding another model. The small
comparison did not establish a factual-accuracy advantage for NuExtract3.

[GLiNER2.5 base](gliner2.5-base/README.md) remains an assessed English alternative,
outside the recommendations: fast span extraction at **0.87 GiB peak RSS** in
the clearer-schema test, but it filled absent fields and included unwanted records.

## Reranking

**Query and candidate passages → relevance order.** Run search first, then
use a reranker to choose which passages to send to an assistant. It cannot
recover a document search missed. Scores compare candidates, not answer confidence.

| Choice | Choose it for | Resource tradeoff | License |
|---|---|---|---|
| [Ettin 150M](ettin-reranker/README.md) | English search with a small CPU footprint; stronger ordering in our English sample. | Q8_0, CrispEmbed/CPU: **194 MiB RSS**, **0.76 s** median for 20 candidates. | [Apache-2.0][apache] |
| [Qwen3 Reranker 0.6B](qwen3-reranker/README.md) | Multilingual retrieval; local evidence covers English and one Swedish-query check. | Q8_0, llama.cpp: CPU **2.47 GiB / 4.27 s**; GPU **1.32 GiB / 0.79 s**, for 20 candidates. | [Apache-2.0][apache] |

Give Qwen the GPU for shorter waits when it is available between other steps.
Larger Ettin 400M and Qwen 4B alternatives did not earn their extra cost in the
sample; their cards retain those comparisons.

## Translation

**Source text and a language pair → translated text.** Choose by the languages
you use, available memory and license. Review names, quantities, negation and
required actions before another step relies on the translation.

### Tiny Aya: choose by language

All four instruction-tuned variants use
**[CC BY-NC 4.0 · Noncommercial][noncommercial]**. Each official Q8_0 build is
**3.57 GB** of weights, with llama.cpp/GPU selected and CPU available.

| Choice | Language focus, with examples | Evidence and memory |
|---|---|---|
| [Global](tiny-aya/README.md#choose-a-variant) | Balanced across regions; mixed language families. | Six local translation cases; **3.78 GiB peak RSS**. |
| [Water](tiny-aya/README.md#choose-a-variant) | Europe and Asia-Pacific: Swedish, German, French, Chinese, Japanese, Korean. | Six local translation cases; **3.79 GiB peak RSS**. |
| [Fire](tiny-aya/README.md#choose-a-variant) | South Asia: Hindi, Bengali, Gujarati, Marathi, Tamil, Telugu, Nepali. | Publisher recommendation; GPU request check only. Comparable memory/quality unmeasured. |
| [Earth](tiny-aya/README.md#choose-a-variant) | West Asia and Africa: Arabic, Persian, Turkish, Hebrew, Swahili, Amharic, Hausa, Yoruba. | Publisher recommendation; one Swahili output changed Tuesday to Monday. Comparable memory and wider quality unmeasured. |

Regional focus is an emphasis within the family's advertised **70+ languages**.
Water is a promising European-language starting point in our small sample.
Global and Water took about 1–2 seconds for short requests; their measurements
do not transfer to Fire or Earth. Use normal chat instructions naming both languages.

### TranslateGemma: choose by resource budget

The family advertises **55 languages**, uses the **[Gemma license][gemma]**,
and requires source/target language codes through its translation adapter.

| Choice | Reason to choose | Peak server RSS with GPU |
|---|---|---:|
| [4B Q5_K_M](translategemma/README.md) | Lowest measured memory and shortest waits among these translation presets. | **3.00 GiB** |
| [12B Q5_K_M](translategemma/README.md) | Middle resource option; corrected some short errors from 4B but introduced others. | **8.66 GiB** |
| [27B Q4_K_M](translategemma/README.md) | Preserved meaning in all five short cases, with substantially longer waits. | **16.39 GiB** |

All five locally compared Tiny Aya/TranslateGemma builds changed meaning in the
longer procedure. The six-case comparison covers English, Swedish, German and
French, with model-assisted review. It does not rank their wider language sets.
The cards explain the errors, input limits and measured configurations.

## Use the measurements

Measurements are from **M5 / 32 GiB, 4 October 2026**, one model at a time.
The selected configurations use llama.cpp **b11396** or CrispEmbed **0.17.11**;
GLiNER's card identifies its Python runtime. RSS is sampled process memory,
not minimum RAM. Smaller-machine fit and co-residency remain unmeasured.

Loading models sequentially can free memory for the next step. Count loading,
checking and repairs when deciding whether a specialist improves your pipeline.

[General models and memory tiers](../README.md) ·
[Choosing a local model](../../guides/choose-a-local-model.md).

[apache]: https://choosealicense.com/licenses/apache-2.0/
[noncommercial]: https://creativecommons.org/licenses/by-nc/4.0/
[gemma]: https://ai.google.dev/gemma/terms
