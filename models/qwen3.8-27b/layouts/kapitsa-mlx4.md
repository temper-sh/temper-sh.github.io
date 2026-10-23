# Qwen3.8 27B with Kapitsa

**Our recommended Qwen configuration for a 32 GiB Apple Silicon Mac.** The
measured M5 configuration completed a checked Flask change faster than Splash
and used less memory on that task. Other chips are unmeasured; large cold
inputs favor Splash.

**Temper does not offer this configuration yet.** Direct setup uses
[Kapitsa](https://github.com/nace-ai/kapitsa) with the
[tested version, model files and settings](kapitsa-mlx4-reference.md#tested-configuration).
[Splash remains an installable alternative](splash-ud-q4-k-xl.md).

## Why this configuration

The selected profile uses MLX 4-bit weights, DFlash2 draft acceleration,
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

The process counter excludes some system and accelerator-service costs. It
does not establish minimum machine RAM or headroom with other applications.

## Tradeoffs that matter

- **Large cold inputs:** at about 85.6k input tokens, cold lookup took **981 s**
  with Kapitsa versus **608 s** with Splash. Both retrieved the values;
  Splash added a forbidden Markdown fence. Loaded/peak footprint over the
  full lookup/edit/long-output sequence was **15.87/23.99 GiB** versus
  **19.43/23.05 GiB**. Kapitsa's lower loaded memory did not give a lower peak.
- **Conversation reuse:** the same sequence favored Kapitsa for continuing
  after long generated output. The continued question repeated identifiers
  already answered, so this establishes cache reuse, not fresh retrieval or
  coding throughout the window.
- **Review still matters:** an earlier Kapitsa configuration removed required
  return annotations, and a separate pagination task favored Splash's elapsed
  time. The reference retains those outcomes with their configurations.
- **Text only:** images and video were not evaluated. Actual fit and speed on
  other Apple Silicon machines remain unmeasured.

[Configuration, comparisons and measurement limits](kapitsa-mlx4-reference.md) ·
[Qwen model overview](../README.md) · [Explore Temper presets](../../../catalog/README.md).
