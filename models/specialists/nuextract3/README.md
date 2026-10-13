# NuExtract3

**Our assessment:** A useful candidate for local structured extraction from
text and simple document images. Its native template interface produced the
required JSON consistently, with shorter requests than the tested general-model
control. This small comparison does not establish a factual-accuracy advantage
over Qwen3.5 4B with structured output.

**Official:** NuMind's 4B text-and-image extraction model, [Apache-2.0](https://choosealicense.com/licenses/apache-2.0/). Its
interface takes a template describing the desired data and optional extraction
instructions. [Model details](https://huggingface.co/numind/NuExtract3),
[official GGUF builds](https://huggingface.co/numind/NuExtract3-GGUF), checked
4 October 2026.

Tested **Q4_K_M** with the matching **BF16 projector**, revision
`28a1aae6288c5d50bdf999ca2e572240966d669b`, llama.cpp **b11396**, Metal or CPU,
8,192-token context, one slot, thinking off, Q8_0 KV cache, temperature 0 and
1,536 output tokens. Requests used the publisher's template and instructions
interface through Temper's supervised API. CPU placement includes the image
projector; it is much slower for the tested image. These are component checks,
not an integrated document-processing workflow.

## Use in a pipeline

**Text or a document image plus an extraction template → structured records.**
Use the native template to describe field types, and instructions to specify
which records to include. Explicitly request pending records and null values
where appropriate. The [request guide](https://github.com/temper-sh/temper/blob/master/docs/SPECIALISTS.md#extract-structured-records)
shows text and image input. Prefer GPU for images; validate record associations
and absence before the next step consumes the JSON. Local language evidence is
mostly English, with one Swedish case.

## Record correctness

Nine small fictional cases had answers fixed before inference. Checks
required complete records, correct field associations, correct absence and
the requested output shape; valid JSON alone did not pass.

| Task | Observed result |
|---|---|
| Invoice parties and totals; two associated orders | Correct records |
| Approved work only; quotation with no invoice | Correct selection and absence |
| Instructions quoted inside a document | Extracted the source facts |
| Swedish dispatch; amount and date normalization | Correct in one case each |
| Legible invoice image, without a transcript | Correct fields |
| Order awaiting dispatch | Original wording returned no record; explicitly requiring pending orders returned the order ID with null shipping fields |

The explicit-record version of the suite passed **9/9 cases on both CPU and
GPU**. Wording still matters: the original b11381 comparison met the strict
oracle in **8/9 cases**. The remaining question
asked for a “shipment record” although no dispatch had occurred; its oracle
expected the known order with null fields. The clarified follow-up passed and
is a separate observation, not a replacement first answer.

On the same nine cases, [Qwen3.5 4B](../../qwen3.5-4b/README.md) returned correct
facts but used a bare array twice. An ordinary JSON Schema constraint removed
those shape errors and yielded **8/9**, with the same pending-record omission.
The schema supplied types and structure, never expected values or record
counts. A native extraction interface helps predictability; this evidence does
not show that separate specialist weights are necessary for these tasks.

## Performance

Measured **4 October 2026**, Apple M5, **32 GiB**, macOS **26.6.1 (25G76)**,
Each model ran separately with its BF16 projector. The general-model control
retains its original b11381 configuration and question wording.

| Configuration | Eight text requests | Invoice-image request | Peak sampled server RSS |
|---|---:|---:|---:|
| NuExtract3 Q4_K_M, native template, Metal, b11396 | 0.37–1.62 s | 3.04 s | 3.69 GiB |
| NuExtract3 Q4_K_M, native template, CPU, b11396 | 2.10–4.70 s | 144.93 s | 6.31 GiB |
| Qwen3.5 4B Q4_K_M, JSON Schema, Metal, b11381 | 0.41–2.91 s | 3.71 s | 5.07 GiB |

Times include incorrect or incomplete answers and measure full requests after
startup, not time to accepted work. No swap growth was observed. RSS is a
sampled process measurement, not a minimum RAM requirement. One rendered
invoice does not establish robust OCR, real-scan quality, handwriting or complex
document-layout support. Sequential use was tested; benefit to a larger
controller and co-residency were not.

[Specialist models](../README.md).
