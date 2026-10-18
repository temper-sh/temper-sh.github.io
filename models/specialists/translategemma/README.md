# TranslateGemma

**Our assessment:** A local translation family with useful memory choices,
but all three tested sizes made meaning errors. The 4B build is the least
expensive starting point; the larger builds corrected some short examples
without making operational text reliable enough to use without review.

**Official:** Google provides 4B, 12B and 27B translation models covering 55
languages, with a 2K-token input limit, under the
[Gemma license](https://ai.google.dev/gemma/terms).
[Official model details](https://huggingface.co/google/translategemma-4b-it)
and [family report](https://arxiv.org/abs/2601.09012), checked 2026-10-04.
Publisher comparisons do not measure the selected compressed weights.

Tested builds: Bullerwins 4B/12B Q5_K_M and 27B Q4_K_M GGUF, llama.cpp b11396
on Metal, greedy decoding, F16 KV, 4,096-token total context, one slot, four
CPU threads, batch 512/microbatch 256. The tested pinned text-input adapter
preserved the publisher's prompt while accepting source and target language
codes. These are text-only configurations; image translation was not tested.

## Use in a pipeline

**Text plus explicit source/target language codes → translated text.** Use
the embedded publisher template with the catalog's llama.cpp b11716 and pass
the codes on every request; the
[request guide](https://github.com/temper-sh/temper/blob/master/docs/SPECIALISTS.md#translategemma)
shows the contract. Keep input within 2K tokens and check the output before a
later pipeline step relies on it.

Native language-code handling preserves the former adapter's prompt: the
4B check on 2026-10-17 produced identical rendered prompts and translations for
English→French and English→German. All three selected GGUFs contain the same
template bytes. This verifies the request interface; the broader quality and
performance measurements below retain their b11396 configuration.

Choose 4B when memory and waiting time matter most. The 12B and 27B builds
corrected some short errors at greater cost; none made the longer procedure
reliable without review. [Tiny Aya](../tiny-aya/README.md) offers compact
regional language choices under noncommercial terms, with a normal chat
instruction instead of language-code kwargs.

## Translation quality

Six fixed cases covered English→Swedish, Swedish→English, German→English,
French→English and English→German. They tested dates, amounts, negation,
distinct UI actions and a longer maintenance procedure. Meaning was reviewed
against demands written before inference. This was model-assisted review,
without independent bilingual human review or a translation benchmark.

| Task | Observed result |
|---|---|
| Backup schedule and retention | 4B and 27B preserved the reviewed meaning. 12B changed “this week's run” to “today's run.” |
| Restore instruction and quotation amount | All three preserved the reviewed negation, amount and required actions. |
| Postponed delivery | 12B and 27B preserved the subject and date. 4B changed delivery to order. |
| Save, Discard and Cancel | 12B and 27B kept the actions distinct. 4B translated both Discard and Cancel as “Abbrechen.” |
| Maintenance procedure | All three changed operational meaning. 4B lost the retryable-error instruction; 12B changed an unverified completion time to a verified one; 27B narrowed write restrictions to uploads. The success condition for storing and indexing a document also changed. |

The 27B long case first ended without an answer after a Temper process-observation
interruption. It completed after a bounded supervision retry was added, using
the same model, prompt and settings. Its semantic finding applies to that
separately retained follow-up.

## Performance

Measured 2026-10-04 on Apple M5 / 32 GiB, macOS 26.6.1, with llama.cpp b11396
and the settings above. Each family condition used a fresh server. Requests
had no prompt-cache reuse; filesystem cache state was not controlled.

| Build | Weight files | Five short requests | Long document | Peak engine RSS |
|---|---:|---:|---:|---:|
| 4B Q5_K_M | 2.83 GB | 0.77–1.48 s | 20.10 s | 3.00 GiB |
| 12B Q5_K_M | 8.45 GB | 2.36–4.19 s | 58.76 s | 8.66 GiB |
| 27B Q4_K_M | 16.55 GB | 4.46–8.55 s | 115.95 s, follow-up | 16.39 GiB |

Short requests used 104–114 prompt tokens. The long request used 573 prompt
tokens and produced 778–792 tokens. Times include request transport and
inference, excluding model startup. They measure delivery of the returned
translation, including its errors, rather than time to an accepted translation.
RSS is sampled server resident memory, not GPU allocation or minimum system RAM.
No swap growth or resource-limit stop was observed.

Three paired fresh-process 4B checks compared microbatch 256 and 512 on the
long case. Median request times were 21.43 and 21.04 seconds, but 512 did not
consistently improve paired request times and its median prompt processing was
slower: 549 ms versus 533 ms. All six outputs were identical and retained the
same meaning errors. Keep 256 for this preset; this is not tuning evidence for
12B or 27B.

CPU performance, smaller-machine fit, simultaneous use with a main model,
longer inputs and the remaining languages are unmeasured.

[Specialist models](../README.md).
