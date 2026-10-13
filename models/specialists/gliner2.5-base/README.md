# GLiNER2.5 base

**Our assessment:** Fast at extracting explicit English fields, but unsuitable
as a general record-extraction replacement in this comparison. It copied spans
that looked like the requested fields even when the underlying event or record
was absent, cancelled or merely proposed.

**Official:** Fastino's 194M English extraction model based on DeBERTa-v3-base,
[Apache-2.0](https://choosealicense.com/licenses/apache-2.0/). It selects spans from supplied text through a native extraction
interface. [Model details](https://huggingface.co/fastino/gliner2.5-base-v1),
[GLiNER2 interface](https://github.com/fastino-ai/GLiNER2), checked
4 October 2026.

Tested checkpoint `ca906247640776a07753514055be9726f9080ead` through
`AutoExtractor.extract_json`, **gliner2 2.0.0**, Transformers **4.57.6**,
PyTorch **2.14.1**, protobuf **7.36.2**, CPU/four threads, threshold **0.5**.
Missing or empty scalar spans were translated to null without adding a
reasoning or normalization step. No Temper preset is implied by this native
Python configuration.

## Use in a pipeline

**English text and field descriptions → selected text spans.** Consider it
only where later checks can reject unsupported fields and records. It does not
provide the tested normalization, image input or record-selection behavior
needed by the broader extraction task. [NuExtract3](../nuextract3/README.md) is
the current native extraction recommendation.

## Record correctness

Six fictional English record tasks had independent expected answers. Two
schema formulations used the same source facts and fixed threshold: a generic
`records` label with the task repeated in field descriptions, and semantic
record names with concise descriptions. They are separate configurations;
results were not selected case by case.

| Task | Generic record label | Named records |
|---|---|---|
| Invoice parties and total | Correct | Correct |
| Two orders and unrelated warehouse stock | Used stock count 400 as an order quantity | Correct quantities and associations |
| Actual shipping fields for a pending order | Filled absent fields from unrelated spans | Same error |
| Approved work only | Included cancelled and proposed work | Same error |
| Quotation, with no issued invoice | Returned quotation as invoice | Same error |
| Instructions quoted inside an invoice | Correct | Correct |

The named formulation completed **3/6** tasks correctly, versus **2/6** for
the generic formulation. The pending-order question has a record-existence
ambiguity, but copying an order ID into carrier and tracking fields remains
unsupported by its source. Swedish, document images and normalization were
declared unsupported in this configuration and were not scored as failures.

## Performance

Measured **4 October 2026**, Apple M5, **32 GiB**, macOS **26.6.1 (25G76)**,
using the Python stack above, with no other resident inference model.

| Native schema | Six request times | Peak sampled process RSS |
|---|---:|---:|
| Generic label and repeated task descriptions | 0.047–0.106 s | 2.00 GiB |
| Named records and concise descriptions | **0.039–0.064 s** | **0.87 GiB** |

These include wrong answers and exclude model loading. Schema wording changes
the encoded input and therefore belongs to the measured configuration. No swap
growth was observed. The initial environment could not load the tokenizer until
protobuf was installed; that startup failure produced no model answers.

The evidence supports bounded field extraction with explicit validation. It
does not establish reliable record selection, absence detection, wider language
support, minimum RAM or controller work saved.

[Specialist models](../README.md).
