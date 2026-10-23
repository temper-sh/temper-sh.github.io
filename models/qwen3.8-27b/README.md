# Qwen3.8 27B

**For a 32 GiB Apple Silicon Mac, our recommended Qwen configuration is
[Kapitsa with MLX 4-bit weights](layouts/kapitsa-mlx4.md).** The evidence is
from an M5; other chips are unmeasured. It completed a checked Flask change
faster and with less memory than the Splash control, while Splash favored
large cold inputs. The Kapitsa preset requires Temper 0.4.0 or newer.

**Publisher description:** A dense 27B model accepting text, images and video,
with a 262,144-token native context. Apache 2.0.
[Official model details](https://huggingface.co/Qwen/Qwen3.8-27B).

The configurations here use text. Kapitsa uses MLX weights and native
rendering; the offered Splash and llama.cpp presets share Unsloth UD-Q4_K_XL
weights and the Frog template. The publisher's context and modality claims do
not establish those capabilities in these local configurations.

## Configuration choices

| Configuration | Purpose and availability | Observations and details |
|---|---|---|
| Kapitsa | Recommended 32 GiB Qwen configuration for reviewed coding work; MLX 4-bit, DFlash2, thinking off, 98,304-token window. Available in Temper 0.4.0+. | [Recommendation and hardware table](layouts/kapitsa-mlx4.md); slower large cold prefill and long-context memory tradeoffs. |
| Splash | Installable alternative with DFlash2 and Frog integration. Requires M3+, macOS 26.4+ and 32 GiB+. | [Short preset page and hardware table](layouts/splash-ud-q4-k-xl.md). |
| llama.cpp | Installable preset with built-in draft prediction. Document and tool cases were also run through this engine. | [Configuration and evidence](layouts/llama-cpp-reference.md), including errors and context limits. |

[Explore the presets and set up Temper](../../catalog/README.md).

## Coding workload on tested hardware

Kapitsa 0.2.9, MLX affine 4-bit/group-64, DFlash2, ANE on, thinking off;
98,304 configured tokens, 1,024-token prefill chunks and a 24 GiB engine allowance.

| Machine | Work | Outcome and elapsed time | Loaded → peak memory |
|---|---|---|---|
| M5 · 32 GiB | Flask decorator change with bounded corrections | Final behavior, AI-assisted source review and focused typing checks pass; **13m14s**, four responses | **15.87 → 21.14 GiB** engine physical footprint |

This is one checked task, not general coding reliability or human acceptance.
The process counter omits some system costs and does not establish minimum
machine RAM. The [Kapitsa reference](layouts/kapitsa-mlx4-reference.md) retains
the Splash comparison, first-attempt failures, a separate pagination result
favoring Splash and the long-input memory crossover.

## What the document tests observed

The llama.cpp cases used simulated tools and AI-assisted review. The records
identify the engine builds; they do not establish general reliability or human
acceptance.

| Work | Observed result |
|---|---|
| Registration, handover and notice revision | Four reserved jobs completed their checks on b10964. |
| Combining sources and recovering edits | 7/10 jobs completed on b10964; JSON placement, counting and Swedish prose errors remained. |
| Long-input lookup | A b11157 configuration retrieved facts and reused context; larger workloads encountered memory-pressure stops. |

The [llama.cpp reference](layouts/llama-cpp-reference.md) retains measured
latency and memory, methods and failed cases for those document workloads.
Those findings do not transfer to Kapitsa. [Context and caching](tuning.md)
explains the related controls.

[All model records](../README.md) · [Tools and resources](../../tools.md).
