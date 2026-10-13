# Qwen3 rerankers

**Our assessment:** The 0.6B model remains a useful multilingual candidate,
but Ettin 150M was faster and ranked the labelled answers better in the tested
English sample. The 4B model's small quality difference did not justify its
extra cost here. Giving it Metal through sequential execution made it much
faster than CPU placement, without changing that choice.

**Official:** Qwen's 0.6B and 4B reranking models support multilingual,
instruction-aware relevance scoring, under [Apache-2.0](https://choosealicense.com/licenses/apache-2.0/). They reorder supplied
candidates; they do not search a corpus themselves.
[0.6B model details](https://huggingface.co/Qwen/Qwen3-Reranker-0.6B),
[4B model details](https://huggingface.co/Qwen/Qwen3-Reranker-4B), checked
4 October 2026.

Tested the Voodisss **Q8_0** conversions for
[0.6B](https://huggingface.co/Voodisss/Qwen3-Reranker-0.6B-GGUF-llama_cpp/tree/eb9ad47d4e53c2a6abd6158b505f1539d1c5650c)
and [4B](https://huggingface.co/Voodisss/Qwen3-Reranker-4B-GGUF-llama_cpp/tree/1b452c803342e73ac3644551b727dfd51a09fd5b),
using llama.cpp **b11396** for the selected 0.6B CPU/GPU presets and **b11381**
for the 4B comparison. Both use the embedded reranking template, rank pooling,
4,096-token context, one slot, batch/microbatch 256 and four CPU threads.
These conversions include the scoring head expected by llama.cpp; the evidence
does not transfer to arbitrary GGUF conversions or added query instructions.

## Use in a pipeline

**Query and candidate passages → multilingual relevance order.** Use the 0.6B
preset after search, through the [rerank API](https://github.com/temper-sh/temper/blob/master/docs/SPECIALISTS.md#rerank-search-results).
Keep its embedded scoring template and send the plain query. GPU placement is
useful when reranking runs between other model steps; CPU is available when the
GPU is occupied. For English-only work, [Ettin 150M](../ettin-reranker/README.md)
is the smaller CPU choice. Broad language coverage is publisher-reported; local
evidence is the English cohort and one Swedish-query check below.

## Ranking quality

All 50 [NanoMSMARCO queries](https://huggingface.co/datasets/zeta-alpha-ai/NanoMSMARCO/tree/7b8ff22f2771dc65ac5b439f222eb19a1f56abda)
used the same top 20 BM25 candidates from 5,043 passages. No labelled answer
was inserted into the pool. Only 38 queries contained their labelled answer,
so the other twelve contribute zero to these full-cohort metrics.

| Ordering | NDCG@10 | MRR@10 | Recall@10 |
|---|---:|---:|---:|
| BM25 | 0.482 | 0.413 | 0.70 |
| Qwen3 0.6B, CPU | 0.589 | 0.533 | 0.76 |
| Qwen3 0.6B, Metal | 0.591 | 0.536 | 0.76 |
| Qwen3 4B, CPU | 0.599 | 0.546 | 0.76 |
| Qwen3 4B, Metal | 0.599 | 0.546 | 0.76 |
| [Ettin 150M Q8_0](../ettin-reranker/README.md), CPU, CrispEmbed | **0.653** | **0.619** | **0.76** |

The metrics reward earlier placement of labelled answers. The two 4B placements
were measured independently; equality of these aggregates does not mean their
full score vectors or complete rankings were identical. Public benchmark
exposure and sparse labels limit this small sample's generality.

In six fictional English maintenance questions, a direct answer ranked first
in three cases for 0.6B and four for 4B. Both placed the direct answer first
for one Swedish query over English documents. That single case is a narrow
multilingual witness, not broad language validation. Input reversal and a warm
repeat preserved the diagnostic orderings. Neither model's raw score establishes
whether an answer exists: the no-answer case still received positive scores.

## Performance

Measured **4 October 2026**, Apple M5, **32 GiB**, macOS **26.6.1 (25G76)**,
Each configuration ran alone, with no resident main model.

| Configuration | Median request, 20 passages | Peak sampled server RSS |
|---|---:|---:|
| 0.6B Q8_0, CPU, b11396 | 4.268 s | 2.47 GiB |
| 0.6B Q8_0, Metal, b11396 | **0.788 s** | **1.32 GiB** |
| 4B Q8_0, CPU, b11381 | 35.933 s | 10.02 GiB |
| 4B Q8_0, Metal, b11381 | 4.250 s | 4.84 GiB |

The 4B CPU run reached its 30-minute condition deadline after 49 queries.
Its interrupted final query was completed in a separately labelled fresh
process; the table uses one completed request per query across both launches.
Other cohorts completed in one launch. No swap growth was observed.
CPU placement leaves the GPU available; the measured 0.6B GPU route reduces
latency substantially. Small metric differences between placements do not
establish a quality advantage.

Request times include local transport and inference, excluding startup and
BM25 candidate preparation. RSS is sampled process residency, not total GPU
allocation or minimum machine RAM. These short-passage runs do not establish
long-document behavior, useful overlap with a main model, or time saved in a
complete search-and-answer workflow.

[Specialist models](../README.md).
