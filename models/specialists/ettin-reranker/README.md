# Ettin rerankers

**Our assessment:** Ettin 150M is a useful small CPU reranker for English
search. The Q8 build with CrispEmbed used about **194 MiB** of process memory
and retained the native model's ranking quality in this comparison. Native
SentenceTransformers was faster but used about **1.02 GiB**. The 400M model
did not earn its additional cost on this sample.

**Official:** English cross-encoders built on ModernBERT, available here as
150M and 400M checkpoints, under [Apache-2.0](https://choosealicense.com/licenses/apache-2.0/). A cross-encoder scores each query
and candidate passage together; it needs an existing candidate generator.
[150M model details](https://huggingface.co/cross-encoder/ettin-reranker-150m-v1),
[400M model details](https://huggingface.co/cross-encoder/ettin-reranker-400m-v1),
checked 4 October 2026.

The compact configuration uses [CrispEmbed **0.17.11**](https://github.com/CrispStrobe/CrispEmbed/releases/tag/v0.17.11),
CPU/four threads, and the [cstr Q8_0 GGUF](https://huggingface.co/cstr/ettin-reranker-150m-v1-GGUF)
at revision `9c680bcef3a5114a708911462eb110e029ce2b87` (162 MB of weights).
Its fixed limit is 7,999 tokens per query/passage pair; long-document behavior
is unmeasured. Keep the matching scoring head and engine together: an arbitrary
ModernBERT GGUF is not sufficient for this reranker.

Native comparisons used SentenceTransformers **6.1.0**, Transformers **5.18.0**, PyTorch
**2.14.1**, CPU, four threads, batches of four pairs and SDPA attention.
Checkpoint revisions were `025501c4e0f9bbeb4c5b198318e0089ff061cc14` (150M)
and `5dca36282a5d85f368d2544002513a29159b4c9e` (400M).

## Use in a pipeline

**English query and candidate passages → relevance scores and order.** Run
search first, pass its candidates to `/v1/rerank`, then send the selected
passages to the next step. Choose the 150M CrispEmbed preset for a compact CPU
footprint; the native Python comparison is faster but uses more memory.
The [request guide](https://github.com/temper-sh/temper/blob/master/docs/SPECIALISTS.md#rerank-search-results)
shows the input contract. Use [Qwen3 Reranker](../qwen3-reranker/README.md) for
multilingual work. Scores compare supplied candidates; they are not confidence
that the corpus contains an answer.

## Ranking quality

All 50 queries from [NanoMSMARCO](https://huggingface.co/datasets/zeta-alpha-ai/NanoMSMARCO/tree/7b8ff22f2771dc65ac5b439f222eb19a1f56abda)
were searched against its 5,043 passages using BM25. Each model reranked the
same top 20 candidates, with no known answer inserted. Only 38 queries had
their labelled answer in that pool. The other twelve contribute zero to the
metrics below; a reranker cannot recover a passage it never receives.

| Ordering | NDCG@10 | MRR@10 | Recall@10 |
|---|---:|---:|---:|
| BM25 candidate order | 0.482 | 0.413 | 0.70 |
| Ettin 150M Q8_0, CrispEmbed | **0.653** | **0.619** | **0.76** |
| Ettin 150M, SentenceTransformers | 0.652 | 0.617 | 0.76 |
| Ettin 400M, SentenceTransformers | 0.635 | 0.594 | 0.76 |
| Qwen3 Reranker 0.6B Q8_0, CPU | 0.589 | 0.533 | 0.76 |

NDCG and reciprocal rank reward placing the labelled answer earlier; recall
counts whether it reaches the first ten. All queries completed. This small
public sample has sparse relevance labels and possible benchmark exposure;
the small GGUF/native difference does not establish a quality advantage, and
the difference between sizes is not evidence of universal superiority.

In six separate fictional English maintenance questions, both sizes placed a
direct answer first in four cases and produced identical orderings. CrispEmbed's
Q8 build preserved those orderings across both input orders and repetitions. Relevant
diagnostic context sometimes ranked above the requested fix. Reversing input
order and repeating requests are retained as separate checks. A no-answer
case still received high scores: these scores are not calibrated confidence
or an abstention rule. Swedish was outside the tested English configuration.

## Performance

Measured **4 October 2026**, Apple M5, **32 GiB**, macOS **26.6.1 (25G76)**.
Each model ran separately, with no resident main model.

| Configuration | Median request, 20 passages | Peak sampled process RSS |
|---|---:|---:|
| Ettin 150M Q8_0, CPU, CrispEmbed 0.17.11 | 0.759 s | **194 MiB** |
| Ettin 150M, CPU, SentenceTransformers | **0.388 s** | 1.02 GiB |
| Ettin 400M, CPU, SentenceTransformers | 1.021 s | 1.96 GiB |
| Qwen3 Reranker 0.6B Q8_0, CPU, llama.cpp b11381 | 4.004 s | 2.47 GiB |

Request time includes tokenization and scoring, excluding model load and BM25
candidate preparation. CrispEmbed reached readiness in 0.27 s. Fresh Python
process load took 2.26 s for 150M and 1.99 s for 400M; filesystem cache state
was uncontrolled. No swap growth was
observed. RSS is a sampled process measurement, not minimum system RAM.

This measures short-passage reranking. Long documents, non-English retrieval,
controller overhead, co-residency and time to a correct final answer remain
unmeasured.

[Specialist models](../README.md).
