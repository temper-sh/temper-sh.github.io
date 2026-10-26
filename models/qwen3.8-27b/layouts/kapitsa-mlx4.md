# Qwen3.8 27B with Kapitsa

**Parked experimental layout. Not recommended or offered in the current
Temper catalog.** On a 64 GiB M5 Pro, the tested configuration submitted
incorrect pagination and revision-store repairs and failed a held-out
streaming repair, omitting the required tests. Lower memory use and successful
context/cache trials do not resolve those coding-quality concerns.

The successful 32 GiB Flask task remains useful evidence, but does not support
a general recommendation. The comparison changes weights, prompt rendering,
KV precision and engine together; it does not prove that Kapitsa itself causes
the failures. Splash also failed the held-out repair.

Existing saved `qwen3.8-27b-mlx4-kapitsa` presets remain user-owned under
**Other** after a catalog update. [Splash](splash-ud-q4-k-xl.md) and
[llama.cpp](llama-cpp-reference.md) remain offered.

## Tested configuration

The measured profile uses MLX 4-bit weights, DFlash2 draft acceleration,
thinking off and a **98,304-token window**. A 24 GiB engine allowance and
1,024-token prefill chunks leave more room for working buffers than the larger
chunk setting tested. Neither the allowance nor the configured window proves
minimum RAM or useful work at every context length.

On the Flask task, Kapitsa completed in **13m14s**, versus **19m37s** for the
thinking-off Splash control. Both final patches passed behavior checks,
AI-assisted source review and a focused typing check. Loaded/peak engine
physical footprint was **15.87/21.14 GiB** for Kapitsa and **19.43/22.03 GiB**
for Splash. Both required corrections; these are bounded test outcomes, not
human acceptance or a general coding-speed ranking.

## Behavior on tested hardware

Kapitsa 0.2.9, MLX affine 4-bit/group-64 target weights, DFlash2, Apple Neural
Engine (ANE) enabled,
thinking off, one slot and no disk cache.

| Machine | Work | Outcome | Elapsed time | Loaded → peak memory |
|---|---|---|---:|---|
| M5 · 32 GiB | Flask decorator change; selected 98,304-token configuration | Final patch passed behavior, source review and focused typing checks | **13m14s**, four responses | **15.87 → 21.14 GiB** engine physical footprint |
| M5 Pro · 64 GiB | Held-out streaming repair; same 98,304-token configuration | Three independent test failures and four errors out of ten; required test module missing | **99.07 s**, one response | **15.87 → 18.92 GiB** engine physical footprint |

These are different tasks, not a controlled hardware comparison. The process
counter excludes router and separate ANE-service costs. It does not establish
minimum machine RAM or headroom with other applications.

## Tradeoffs that matter

- **Large cold inputs on the 32 GiB M5:** at about 85.6k input tokens, cold
  lookup took **981 s** with Kapitsa versus **608 s** with Splash. Both retrieved the values;
  Splash added a forbidden Markdown fence. Loaded/peak footprint over the
  full lookup/edit/long-output sequence was **15.87/23.99 GiB** versus
  **19.43/23.05 GiB**. Kapitsa's lower loaded memory did not give a lower peak.
- **Conversation reuse:** the same sequence favored Kapitsa for continuing
  after long generated output. The continued question repeated identifiers
  already answered, so this establishes cache reuse, not fresh retrieval or
  coding throughout the window.
- **Coding quality:** on the 64 GiB machine, passing model-authored tests hid
  confirmed contract violations in pagination and revision-store repairs.
  The held-out repair failed independent tests and omitted a deliverable.
  The reference retains checks, timings and memory for both engines.
- **Text only:** images and video were not evaluated. Actual fit and speed on
  chips beyond the measured M5 and M5 Pro remain unmeasured.

[Configuration, comparisons and measurement limits](kapitsa-mlx4-reference.md) ·
[Qwen model overview](../README.md) · [Explore Temper presets](../../../catalog/README.md).
