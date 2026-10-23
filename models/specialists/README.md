# Specialist model records

Specialists expose a defined input/output job. The linked records describe
publisher coverage, local observations and resource costs separately. The small
local samples do not establish reliability across languages or workflows.

[Explore the specialist presets](../../catalog/README.md#specialists).
Your application connects the steps; a layout makes the models available to load
when requested. The [request guide](https://github.com/temper-sh/temper/blob/master/docs/SPECIALISTS.md)
describes their different APIs.

## Structured extraction

**Text or document image → records matching a schema.** Check missing values
and the association between fields, as well as whether the output is valid JSON.

| Model | Interface and coverage | Record |
|---|---|---|
| NuExtract3 4B | Schema, instructions and text or images; GPU and CPU configurations. | [Extraction cases and resource measurements](nuextract3/README.md). |
| GLiNER2.5 base | English span extraction; assessed outside the offered presets. | [Schema-dependent observations](gliner2.5-base/README.md). |

A general assistant can also produce structured records. The
[Qwen3.5 comparison](../qwen3.5-4b/README.md#structured-extraction) did not
establish a factual-accuracy advantage for NuExtract3 in its small sample.

## Reranking

**Query and candidate passages → relevance scores.** Search must retrieve the
candidates first. Scores order those candidates; they are not answer confidence.

| Model | Configuration | Record |
|---|---|---|
| Ettin 150M | English; CrispEmbed on CPU. | [Ranking cases and conversion comparison](ettin-reranker/README.md). |
| Qwen3 Reranker 0.6B | Multilingual model; llama.cpp on CPU or GPU. | [English sample and Swedish-query check](qwen3-reranker/README.md). |

## Translation

**Text and a language pair → translated text.** Publisher language lists are
broader than our local checks. Review names, quantities, negation and required
actions before another step relies on the output.

| Family | Variants and interface | Record |
|---|---|---|
| Tiny Aya | Global, Water, Fire and Earth emphasize different language regions. Chat instructions name the languages. Noncommercial terms. | [Regional coverage, license and local cases](tiny-aya/README.md). |
| TranslateGemma | 4B, 12B and 27B configurations; explicit source/target language codes. Gemma terms. | [Size comparisons, license and meaning errors](translategemma/README.md). |

The local translation comparison used model-assisted review of six cases.
All five compared Global/Water/TranslateGemma builds changed meaning in the
longer procedure; those results do not rank their wider language coverage.

## Resource use

The cards retain configurations, request times, loaded/peak memory where
available and the named counters. Smaller-machine fit and simultaneous use
with another model remain unmeasured. Include loading, checking and repairs
when evaluating a pipeline.

[General model records](../README.md) ·
[Models, tasks and hardware](../../guides/choose-a-local-model.md).
