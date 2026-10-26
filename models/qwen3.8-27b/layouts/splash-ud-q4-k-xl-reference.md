# Qwen3.8 27B / Splash: technical reference

Configuration details and evidence behind the [short preset page](splash-ud-q4-k-xl.md).
For installation, use Temper's published catalog; the evaluated variants here
are not all offered presets. See [how to use the technical references](../../../guides/technical-reference.md).

**Preset ID:** `qwen3.8-27b-q4xl-splash`.

The [Kapitsa reference](kapitsa-mlx4-reference.md) documents the thinking-off
comparison of completed code changes, long-input latency and loaded/peak
memory. Kapitsa is parked experimental after coding failures; Splash remains
offered. Both thinking-off configurations failed a held-out streaming repair,
so the comparison does not establish general coding reliability.
Results for the medium-thinking preset retain
their own settings and do not inherit the thinking-off outcomes.

## Why these choices

| Choice | Decision basis | What the evidence does not establish |
|---|---|---|
| Splash with DFlash2 | Shorter first submissions than llama.cpp/MTP in the two matched coding tasks below, with patches that could be tested and reviewed. | General quality parity, time to accepted code, or how much of the difference comes from the engine versus the draft method. |
| Unsloth UD-Q4_K_XL GGUF | Keeps the exact target weights used by the llama.cpp reference. The tested packaged Splash Q4 configuration produced a syntax error and an invalid patch submission; the GGUF combination produced testable patches. | A quantization-only advantage: the packaged configuration also used a different engine build. Its memory costs were not retained in this comparison. |
| Frog v22.5 | Retains the baseline's message formatting and reasoning controls across engines. Native checks verified identical rendered prompts and token IDs for both coding tasks. | Superiority over the embedded template. This comparison held Frog fixed; the GGUF/Sharp trials were cancelled before inference. |
| 112,000-token window under a 24 GiB allowance | Splash 1.3.2 refused 118,000 under the same allowance; 112,000 admitted and passed the short checks. The measured loaded/peak footprint was 19.46/20.46 GiB. | Maximum usable context, full-window quality or a worst-case memory bound. The 24 GiB allowance is a selected operating budget, not a measured optimum. |
| Int8 conversation cache, medium thinking and sampling defaults | Retained settings of the tested configuration. | This evidence does not isolate why these values outperform other precisions, reasoning modes or sampling values. Independent optimization of these values is not established by these records. |

The [matched coding evidence](#m5--32-gib-matched-coding-comparison-with-llamacpp)
retains the quality findings, request times, memory counter and exact identities.
It also explains the confounds in the packaged-weight comparison. A missing
comparison is not a reason to claim another option is worse.

### Why retain Splash 1.3.2

Splash 1.4.0 was evaluated but not selected for this preset: the matched release
check did not show a request-time improvement, and preserving Frog required a
local frontend patch. Retaining 1.3.2 avoids adopting that additional patch
without a demonstrated benefit in the matched workload.

The release check used M5 / 32 GiB / macOS 26.6.1, the same Unsloth target and
Frog v22.5, a 112,000-token allocation, FP32 recurrent states, int8 conversation
cache and no persistent disk cache. The 1.4.0 engine was the stock release;
its frontend was patched to accept the template override. The measurements
were recorded on 21 October 2026.

| Matched release check | Splash 1.3.2 | Splash 1.4.0 with template patch |
|---|---:|---:|
| Interface, history, decode, source-audit and repair checks | 14 passed | 14 passed |
| Source-audit request | 31.41 s | 31.86 s |
| Atomic-repair request, passed visible and independent tests | 88.66 s | 91.07 s |
| Engine physical footprint, loaded → peak over the whole check sequence | 19.47 → 22.10 GiB | 19.42 → 22.10 GiB |
| Swap growth | 0 | 0 |

Memory peaks cover the complete check sequence, not just the two timed requests
shown here. These counters are not minimum machine RAM. Automatic ANE placement
differed, and first-start calibration prevents a matched startup comparison.
Separate tuning and persistent-cache experiments within 1.4.0 were not matched
against equivalent tuned 1.3.2 configurations. This decision therefore concerns
the release change and template-maintenance cost, not every possible 1.4.0 setup.

## Setup and integration

The preset requires Apple M3 or newer, macOS 26.4+ and at least 32 GiB.
Temper supplies the Frog template through the Python server's tokenizer
override. Selecting the llama.cpp alternative reuses the target GGUF; Splash's
first startup also prepares a native weight cache, adding time and disk use.
The tested profile calibrates ANE prefill during startup and disables SSD
prefix caching. The engine memory allowance is a cap, not reserved memory.

## Scope of the observations

Splash 1.3.2 short interface checks on M5 / 32 GiB and coding/lookup trials on
M5 Pro / 64 GiB used the same target weights and template. The earlier matched
coding comparison used Splash source build fdc11c5. Its first-submission times
do not describe Splash 1.3.2.

The 32 GiB checks used inputs up to 522 tokens. Additional 64 GiB trials reached
a full 65,536-token allocation, but did not fill the 131,072-token window.
Cold lookup and cache behavior beyond roughly 64k occupied context remain
unmeasured. Configured capacity does not establish useful work at that size.

No Splash 1.3.2 run reported swap growth, a thermal warning or a resource stop.
Engine physical footprint excludes the frontend and router; system wired-memory
growth includes other applications and can miss peaks between samples about
five seconds apart. Neither is minimum machine RAM, and the counters must not
be added. Loaded/peak values remain attached to their individual workloads below.

| M5 / 32 GiB workload | Outcome | Time | Loaded → peak memory |
|---|---|---|---|
| Six short interface/reuse requests; 112,000-token allocation, up to 512 output tokens per request | Passed; largest input 522 tokens | Prefix check: 2.60 s cold / 0.36 s reused | 19.46 → 20.46 GiB engine physical footprint |

## Offered preset and evaluated variants

The offered preset and the evaluated 64 GiB configuration use the same weights,
draft model and precision. The latter allocates a larger context and more
runtime headroom.

| Setting | Published preset · 32 GiB+ | Evaluated variant · 64 GiB |
|---|---:|---:|
| Input + output window | 112,000 tokens | 131,072 tokens |
| Engine memory allowance | 24 GiB | 48 GiB |
| Request timeout | 30 minutes | 4 hours |

The larger window adds 19,072 tokens (17%). The engine allowance is a cap,
not reserved or observed memory. These settings establish neither minimum RAM
nor maximum usable context. A 118,000-token allocation was refused on the
32 GiB Mac under its 24 GiB allowance.

The shared configuration is Splash 1.3.2, Unsloth UD-Q4_K_XL, Frog v22.5,
DFlash2 and int8 conversation cache. Defaults are medium thinking, temperature
1, top-p 0.95, top-k 20 and neutral penalties. Responses, including reasoning,
can use the context remaining after input; there is no separate output cap.
Thinking-off results use a different reasoning setting from the default preset.

## Workload conditions and review findings for Splash 1.3.2

The M5 checks were measured on 17 October 2026; the M5 Pro runs on
20 October 2026.

The M5 / 32 GiB machine ran macOS 26.6.1. The M5 Pro / 64 GiB machine ran
macOS 26.7.1 with a 53,084 MiB Metal budget. Wired-memory growth is relative
to each trial's baseline. The chips, workloads and memory counters differ;
these runs do not isolate a benefit from doubling RAM.

Loaded memory was recorded after the first response on M5, and after engine
readiness on M5 Pro. The M5 short checks allowed up to 512 output tokens per
request. The main table rounds coding time to the nearest second. The M5 Pro
details below retain recorded precision; all memory values are loaded/peak
system wired-memory growth in GiB.

| M5 Pro workload | Actual tokens processed and outcome | Elapsed time | Loaded → peak memory |
|---|---|---:|---:|
| Coding, thinking on, 131,072 window | First response: 16,369 input + 43,375 output including reasoning. Second: 20,846 input + 4,018 output. Submitted code failed independent tests. | 1,372.72 s | 19.22 → 21.63 GiB |
| Coding, thinking off, 131,072 window | Eight responses; final input 53,091 and output 2,713. Proposal passed tests; review concern remained, no final submission. | 450.97 s | 19.27 → 22.65 GiB |
| Coding, thinking off, 65,536 window | Eight responses; reached 63,881 input + 1,655 output, then stopped at the context limit. | 933.07 s | 19.21 → 22.90 GiB |
| Lookup, thinking off, 65,536 window | Correct 32-identifier answer from 14,107 input + 506 output. | 32.23 s | 19.31 → 20.00 GiB |

Coding runs allowed visible tests and repairs, followed by independent tests
and source review. The thinking-on submission passed original and candidate
tests but failed eight of 14 independent checks: teardown ran at the wrong
point in the stream lifetime.

The thinking-off 131,072 run's last tested proposal passed the original,
independent and candidate groups. Source review found a context-restoration
concern; the harness exhausted its eight responses before final submission.
This is useful coding evidence with remaining review work, not completed
delivery. Review, repair and stopping decisions depend on the model and the
agent using it.


## M5 / 32 GiB: matched coding comparison with llama.cpp

The **25–26 September 2026** comparison used an earlier Splash source build on
Apple M5 / 32 GiB / macOS 26.6.1. It supports the first-submission comparison;
its timings do not describe Splash 1.3.2 or the M5 Pro.

| Task | Input tokens | llama.cpp time to submission | Splash time to submission | Splash output tokens |
|---|---:|---:|---:|---:|
| Fix async streaming | 16,078 | 102m 21s | 40m 27s | 37,455 |
| Extend template decorators | 23,398 | 58m 23s | 19m 21s | 21,902 |

Combined request time fell by **62.8%**. Times include input processing and
reasoning, excluding startup and subsequent review. Each task was one first
attempt with seed 17 and no repair feedback. Repair sessions were not timed,
so time to accepted code remains unknown.

| Memory observation | Splash | llama.cpp |
|---|---|---|
| Peak memory during these tasks | **20.3–20.5 GiB** native allocations | Comparable measurement not available here |
| Loaded memory | Not reported | Not reported |

Splash's native allocation counter includes allocations process RSS misses.
No swap growth or resource stop was observed. These values do not establish
minimum RAM or a memory-use advantage over llama.cpp; smaller machines were
not tested.

Both configurations passed the original and independent behavioral checks.
Both bug submissions failed two of their own tests. Splash's failing assertions
concerned when a streamed exception is raised and an inherited double-close
behavior. A separate review probe found that its patch suppressed a
context-cleanup error outside the intended streaming fix. The llama.cpp patch
preserved that error guard and used a narrower mechanism.

Splash's feature passed all 175 checks, including 26 authored tests. Review
found an unnecessary second naming argument across six decorator APIs. Static
typing was not checked. Reviewers knew candidate identities and timings;
two tasks do not establish general quality parity.

### Tested configurations

Both engines received identical rendered prompts and token IDs, the same
GGUF target and Frog template. llama.cpp **b11157** used embedded MTP with
three draft tokens and a 120,000-token ceiling; Splash used DFlash2 and
118,000. This compares the complete configurations.

| Component | Splash configuration |
|---|---|
| Target weights | [Unsloth Qwen3.8 27B UD-Q4_K_XL GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF/tree/4ca720788d1e01f1bff70c033e0d0028fd02e502), Dynamic 3.0 |
| Engine | [Splash source fdc11c5](https://github.com/incoai/splash/tree/fdc11c5965313d7c0648c8be9b02ea385727fdd7) |
| Chat template | [Froggeric v22.5](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates/blob/855bffc49448e299789730ff92c9b8d834d6cc14/chat_template.jinja), supplied through the Python server's tokenizer override |
| Draft model | [Qwen3.8 27B DFlash2](https://huggingface.co/incoai/Qwen3.8-27B-DFlash2/tree/dedf8df68adfb1afeaf7b7480c0a0243108177b4), prepared by Splash |
| Request settings | Medium reasoning, previous thinking omitted; temperature 1, top-p 0.95, top-k 20 |
| Runtime settings | Language only, int8 conversation cache, 24G Metal allocation limit, 118,000-token total ceiling |

The GGUF preserves Unsloth's mixture of quantization formats and precisions.
DFlash2 is a separate draft model proposing tokens for the target to verify.
The largest observed sequence was 53,533 tokens including reasoning and output;
the 118k allocation does not qualify 118k input, multi-turn coding or vision.

A different Q4 weight package produced a syntax error and an invalid patch
submission in the corresponding Frog tests. Its engine build also differed,
so the comparison does not isolate weight compression as the cause.


[Model card](../README.md) · [Choose a model](../../../guides/choose-a-local-model.md).
