# Tiny Aya

**Our assessment:** Recommended compact language helpers for personal,
noncommercial pipelines. Choose Global for mixed regions, or Water, Fire or
Earth for their language emphasis. Water preserved more of the locally tested
instructions than Global, but both changed operational meaning in a longer
procedure. Fire and Earth's regional recommendations follow Cohere; broader
language quality is unmeasured, with one observed Swahili weekday error in Earth.

**Official:** Cohere Labs provides 3.35B instruction models covering 70+ languages,
with an 8K input limit, under
[CC BY-NC 4.0 · Noncommercial](https://creativecommons.org/licenses/by-nc/4.0/)
and Cohere's [acceptable-use policy](https://docs.cohere.com/docs/cohere-labs-acceptable-use-policy).
The four instruction-tuned variants support text generation, summarization and
translation. The regional model cards below were checked 2026-10-04; our local
quality comparison covers translation only.

## Choose a variant

| Variant | Intended language focus | Evidence for this choice |
|---|---|---|
| [Global](https://huggingface.co/CohereLabs/tiny-aya-global) | Balanced across regions; use when mixing language families. | Cohere's positioning, plus six local translation cases. |
| [Water](https://huggingface.co/CohereLabs/tiny-aya-water) | Europe and Asia-Pacific, including Swedish, German, French, Chinese, Japanese and Korean. | Cohere's positioning; local quality checks cover English, Swedish, German and French. |
| [Fire](https://huggingface.co/CohereLabs/tiny-aya-fire) | South Asia, including Hindi, Bengali, Gujarati, Marathi, Tamil, Telugu and Nepali. | Cohere's regional recommendation; local language quality unmeasured. |
| [Earth](https://huggingface.co/CohereLabs/tiny-aya-earth) | West Asia and Africa, including Arabic, Persian, Turkish, Hebrew, Swahili, Amharic, Hausa and Yoruba. | Cohere's regional recommendation; one Swahili output changed the weekday. Wider quality unmeasured. |

These are regional emphases within the family's shared multilingual coverage.
The examples help choose a variant; Cohere's
[language list](https://huggingface.co/CohereLabs/tiny-aya-global#model-details)
names the wider coverage. Performance can differ by language pair. The base
pretrained checkpoint is outside these instruction-model recommendations.

## Use in a pipeline

Send the source text in a normal chat instruction naming both languages, for
example: “Translate the following text from English into Swedish. Return only
the translation.” No TranslateGemma language-code adapter is needed. Keep the
source alongside the output so names, quantities, negation and required actions
can be checked before another step uses the translation.

Temper selects the official **Q8_0** GGUF for each variant, **3.57 GB** per
model, with **llama.cpp b11396**, GPU by default and CPU available. The default
is a **4,096-token total window** with **2,048 output tokens**; the catalog
allows up to 8,192 total tokens, without claiming local capacity validation
there. CPU speed, longer inputs and simultaneous use with a main model remain
unmeasured.

Global/Water tested builds: official Cohere Labs Q8_0 GGUFs with llama.cpp b11396 on Metal,
the embedded chat template, greedy decoding, F16 KV, 4,096-token total context,
one slot, four CPU threads and batch 512/microbatch 256. Each request used one
fixed instruction naming the source and target language and asking for only the
translation. These settings establish the measured baseline; tuning was not
compared.

## Translation quality

Six fixed first attempts covered English→Swedish, Swedish→English,
German→English, French→English and English→German. Dates, amounts, negation,
UI actions and a longer maintenance procedure were checked against semantic
demands written before inference. Review was model-assisted, without an
independent bilingual human reviewer or a translation benchmark.

| Task | Global | Water |
|---|---|---|
| Backup schedule, postponed delivery and quotation amount | Preserved the reviewed meaning. | Preserved the reviewed meaning. |
| Restore instruction and escalation | Preserved the stop conditions but changed the on-call team to “the desk.” | Preserved the reviewed meaning, including the on-call team. |
| Save, Discard and Cancel | Kept the three actions distinct with conventional German labels. | Kept the action outcomes distinct, but used nonstandard labels for Discard and Cancel; terminology needs editing. |
| Maintenance procedure | Changed “stop the rollout” to “stop the rollback,” lost the queue limit and mistranslated the on-call role. | Preserved stopping the rollout, but mistranslated the on-call role and obscured the queue-limit terminology. |

Both preserved the requirement that an upload succeeds only after its document
is stored **and** its index update is acknowledged. All three tested
[TranslateGemma builds](../translategemma/README.md) changed that condition.
Tiny Aya's different errors prevent treating this one improvement as a general
quality ranking. Its awkward Swedish also needs editorial review.

Fire and Earth each completed a separate GPU load/request/shutdown check on
the same machine and engine, using their official Q8_0 builds and the same 4K
settings, with a 256-token response cap. Both translated a two-sentence English
backup instruction, into Hindi and Swahili respectively. No Hindi quality
conclusion was drawn. Earth rendered “every Tuesday” as “kila Jumatatu”:
**Monday**, where Tuesday is **Jumanne**, according to the
[University of Kansas Swahili glossary](https://olrc.ku.edu/swahili-glossary).
This is a post-hoc counterexample, not a scored regional comparison. Request
completion alone did not establish a correct translation.

## Performance

Measured 2026-10-04 on Apple M5 / 32 GiB, macOS 26.6.1, with llama.cpp b11396
and the settings above. Each variant ran sequentially in a fresh server, with
no prompt-cache reuse. Filesystem cache state was not controlled.

| Build | Weight file | Five short requests | Long document | Peak engine RSS |
|---|---:|---:|---:|---:|
| Global Q8_0 | 3.57 GB | 1.06–1.88 s | 21.98 s | 3.78 GiB |
| Water Q8_0 | 3.57 GB | 1.06–1.89 s | 22.11 s | 3.79 GiB |

Cold server readiness took 8.64 seconds for Global and 7.48 seconds for Water.
Short requests used 408–416 prompt tokens and returned 26–54 tokens; the long
request used 863 prompt tokens and returned 719/723 tokens. Request times include
transport and inference, excluding startup. They measure the returned text,
including its errors, rather than time to an accepted translation. The models
used different templates and tokenizers from TranslateGemma.

All twelve requests completed. No swap growth or resource-limit stop was
observed. RSS is sampled server resident memory, not GPU allocation or minimum
system RAM. Smaller-machine fit, other quantizations and wider language quality
remain unmeasured. Fire and Earth do not inherit Global or Water's measurements.

[Specialist models](../README.md).
