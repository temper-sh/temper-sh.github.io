# Model catalog

Models assessed for extraction, writing, editing and tool use. Each card
describes the tested configuration, observed results and limitations. Some
entries document setups that did not work well for the tested tasks.

Use the [model-choice guide](../guides/choose-a-local-model.md) for a starting
point by task. To install, choose from
[Temper's presets](https://github.com/temper-sh/temper/blob/master/catalog/README.md).
The configurations below describe measurements; current presets may use
different settings or software. [The basics](../guides/basics.md) explains the
memory and configuration terms.

| Tier | Machine memory |
|---|---|
| XS | Up to 16 GB |
| S | >16–32 GB |
| M | >32–64 GB |
| L | >64–128 GB |
| XL | >128–256 GB |
| XXL | >256–512 GB |

Tiers describe the estimated memory capacity needed by a configuration.
Placement considers measured server memory and room for the operating system
and other applications. A tier does not establish fit on every machine within
its range, and unified memory and dedicated GPU memory need separate checks.

**All local measurements below used an Apple M5 with 32 GiB.** The smaller
machine placements are estimates; fit on actual 8 or 16 GiB machines remains
unmeasured. Longer contexts, caches and other settings can change the fit.
Unless stated otherwise, the listed configurations use Q4_K_M at 16k context.

## XS · Up to 16 GB

**Estimated fit.** These are the smaller configurations in the catalog. The
larger entries in this group are candidates for the 16 GB end of the range;
placement here does not imply that they all fit an 8 GB machine.

- [Gemma 4 E2B](gemma-4-e2b/README.md) — Field extraction and short tool tasks;
  writing accuracy and recovery from conflicting edits need checking.
- [Gemma 4 E4B](gemma-4-e4b/README.md) — Short drafts from supplied facts and
  tool workflows; longer editing retained errors after feedback.
- [Granite 4.2 3B](granite-4.2-3b/README.md) — Extraction and copyediting;
  reasoning off and low differed when handling another edit to the same file.
- [Granite 4.2 8B](granite-4.2-8b/README.md) — Copyediting and short tool tasks;
  policy summaries and calendar messages needed correction.
- [MiniCPM5 1B](minicpm5-1b/README.md) — Simple field extraction and lookups;
  other tasks needed substantial checking.
- [Ornith 1.5 9B](ornith-1.5-9b/README.md) — Short evidence checks and tool
  workflows; policy interpretation and longer prose needed review.
- [Phi 4 Mini Instruct](phi-4-mini-instruct/README.md) — Extraction and short
  copyediting; the tested setup claimed tool work it had not performed.
- [Qwen3 8B](qwen3-8b/README.md) — Short English drafts, extraction and
  copyediting; some workflows omitted final checks.
- [Qwen3.5 4B](qwen3.5-4b/README.md) — Extraction, classification and short
  tool workflows; longer writing needed close checking.
- [Spark X2.5 4B](spark-x2.5-4b/README.md) — With thinking on, completed short
  policy, copyediting and recovery tasks on official llama.cpp. Broader tests
  on the XHToken fork found limitations in longer writing and translation.

## S · >16–32 GB

These configurations either used more memory or left little room for the rest
of a 16 GB machine. The Olmo placement is conservative: its tested configuration
reached 12.23 GiB of server memory.

- [Olmo 3 7B Instruct](olmo-3-7b-instruct/README.md) — Record lookups and a short
  publishing task with the patched template; outdated instructions and save
  failures remained.
- [Muse Glimmer 30B](muse-glimmer-30b/README.md) — Technical explanations,
  structured drafts and a second review; completed a conflict-recovery workflow.
  Check operational details in plans. UD-Q4_K_XL, medium reasoning, 56k.
- [Gemma 4 26B-A4B](gemma-4-26b-a4b/README.md) — Creative drafting, dialogue,
  tone revision and general explanations, with substantially shorter waits than
  31B on the M5. Unsloth QAT UD-Q4_K_XL, plain decoding, 96k; check factual
  constraints and continuity.
- [Gemma 4 31B](gemma-4-31b/README.md) — Coherent scenes and tone revision,
  with much longer waits; the sample establishes no human preference advantage
  over 26B. Unsloth QAT UD-Q4_K_XL, plain decoding, 40k.
- [Ornith 1.5 35B A3B](ornith-1.5-35b-a3b/README.md) — Following corrections
  and checking evidence; longer workflows stopped before the final report.
  Q4_K_M, thinking budget 512, 32k.
- [Qwen3.8 27B](qwen3.8-27b/README.md) — Recommended for coding with
  [Splash](qwen3.8-27b/layouts/splash-ud-q4-k-xl.md)
  on M5/32 GiB; patches need review. Separate llama.cpp results cover source
  synthesis, document editing and a measured 40k context workload.

## Local fit unmeasured

These assessments used hosted providers. They do not establish local memory
requirements or engine speed.

- [LFM2.5 2.6B](lfm2.5-2.6b/README.md) — Short drafts and structured tasks;
  policy errors remained, and API rate limits prevented some tests. Liquid FP8.
- [Ministral 3 3B](ministral-3-3b/README.md) — Extraction, copyediting and simple
  saved changes; recovery from conflicting edits needed work. Mistral route.
- [Qwen3.8 Flash](qwen3.8-flash/README.md) — Completed two extended tool
  workflows; most notices needed small repairs. Alibaba Cloud route.
- [GLM 5.3 Flash](glm-5.3-flash/README.md) — Completed two extended tool
  workflows; all five notices needed corrections. Z.ai FP8.

## Specialists

[Specialist models](specialists/README.md) are grouped by task, starting with
coding.

[Tools and resources](../catalog/README.md) · [Guides](../guides/README.md) ·
[Back to Temper](../README.md).
