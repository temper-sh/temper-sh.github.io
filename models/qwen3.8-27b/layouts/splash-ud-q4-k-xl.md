# Qwen3.8 27B with Splash

A text-only Qwen preset using Splash. It requires **Apple M3 or newer,
macOS 26.4+ and 32 GiB+**.

**[Set up with Temper](https://github.com/temper-sh/temper#get-started)** ·
[Configuration and evidence](splash-ud-q4-k-xl-reference.md)

For a 32 GiB Mac, [Kapitsa is our recommended Qwen configuration](kapitsa-mlx4.md),
based on M5 measurements. Splash remains the installable alternative and
favored large cold inputs in the comparison. That comparison used thinking
off; the offered Splash preset uses medium thinking.

## Why this combination

**Splash was selected to shorten the wait for a first code patch.** On an M5
with 32 GiB, two coding tasks took **40m 27s and 19m 21s**, versus **102m 21s
and 58m 23s** with llama.cpp. Both configurations needed review and corrections.
Splash peaked at **20.3–20.5 GiB of native allocations**; loaded memory and a
comparable llama.cpp measurement were not recorded. This
[comparison used earlier engine builds](splash-ud-q4-k-xl-reference.md#m5--32-gib-matched-coding-comparison-with-llamacpp)
and measured first submissions, not time to accepted code.

**The 112,000-token setting fits the selected 24 GiB engine allowance.**
Splash 1.3.2 refused 118,000 under that allowance; 112,000 passed startup and
short request checks. That supports the allocation, not useful work across
the full window.

The [decision notes](splash-ud-q4-k-xl-reference.md#why-these-choices) explain
the weight/template baseline, retained defaults and why Splash 1.4.0 was not
selected for this preset.

## Behavior on tested hardware

These observations use **Splash 1.3.2**, Unsloth UD-Q4_K_XL weights, Frog v22.5
and medium thinking. Each row describes one workload.

| Machine | Configuration and work | Outcome | Elapsed time | Loaded → peak memory |
|---|---|---|---:|---|
| M5 · 32 GiB | 112,000-token window; six short interface/reuse requests | Passed; largest input 522 tokens | Prefix check: **2.60 s** cold / **0.36 s** reused | **19.46 → 20.46 GiB** engine physical footprint |
| M5 Pro · 64 GiB | Evaluated 131,072-token window; coding | Submitted code failed independent tests | **22m 53s**, two responses | **19.22 → 21.63 GiB** system wired-memory growth |

The workloads and memory counters differ, so these rows do not compare the
machines' speed or efficiency. Memory observations are not minimum RAM
requirements. Neither trial filled its configured window.

## What to expect

Coding outcomes depend on the task and configuration. The
[thinking-off comparison with Kapitsa](kapitsa-mlx4-reference.md#completed-code-changes)
produced a submitted Flask patch passing behavior, source review and focused
typing checks after corrections. A separate thinking-off trial retained a
source-review concern and stopped before submission; other trials produced
errors that ordinary tests missed. These outcomes do not establish general
coding reliability or apply unchanged to the offered medium-thinking preset.

The larger 64 GiB variant is evaluated separately.
[Technical details](splash-ud-q4-k-xl-reference.md) retain the settings, other
workloads and review findings.

[Qwen model overview](../README.md) · [Explore presets](../../../catalog/README.md).
