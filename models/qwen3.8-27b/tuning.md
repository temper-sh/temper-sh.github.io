# Tuning Qwen3.8 27B

Temper's presets already include selected settings. This optional guide explains
their tradeoffs for readers who want to adjust a configuration themselves.

These notes explain how context, conversation caching and reasoning settings
affect memory use and response time. They cover llama.cpp **b10964 / v0.4.1**
with the **Frog v22.5** chat template. The [model card](README.md) gives the
measured configurations and their limits.

For the recommended coding setup, use the
[Splash configuration and controls](layouts/splash-ud-q4-k-xl.md).
The llama.cpp checkpoint, cache and MTP settings below apply to the engine
named here; Splash uses its own memory controls and a separate DFlash2 draft.

## Context and memory

The context window (`--ctx-size`) holds the document, conversation history,
reasoning and answer together. Leave enough room for the model to finish its
answer and handle follow-ups.

The engine also retains conversation state and checkpoints: saved positions
it can resume from after an edit. Keeping fewer can free memory for longer
input, at the cost of reprocessing more history.

K/V cache precision is a separate memory setting from model-weight
compression. Check answer quality when reducing it, and test both a full
document and a follow-up. A timed-out request does not establish the maximum
usable context.

## Checkpoints and conversation caching

| Setting | Effect |
|---|---|
| `--ctx-checkpoints` | Number of saved conversation positions. |
| `--checkpoint-min-step` | Spacing between those positions. |
| `--cache-ram` | Extra RAM for retaining conversation state when switching threads. |

More checkpoints can reduce reprocessing after edits, but increase the memory
needed to retain each thread. The RAM cache needs room for the thread you
leave and the one you return to; saving one can otherwise evict the other.
Turning extra cache RAM off still permits reuse of the active conversation.

Compare three actions: appending a message, editing an earlier message and
returning after switching threads. A cached prefix avoids repeated input
processing, while generating the answer still takes time. An earlier edit
may require reprocessing from a checkpoint.

Unloading or restarting the engine loses its in-memory state. Include idle
unloading when comparing memory use and response time. Extra cache RAM does
not reduce the cost of reading the input for the first time.

## Reasoning and output allowance

Reasoning and the delivered answer share the output allowance. If reasoning
uses all of it, increase the allowance or lower the reasoning effort. Compare
effort settings with the same sampling values, checking correctness and total
time. Frog v22.5 treats `high` and `xhigh` as equivalent.

If the model keeps following superseded instructions,
`--no-reasoning-preserve` omits earlier reasoning while retaining delivered
answers. Test work that depends on earlier turns before adopting the setting.

## Input processing and generation

Batch and microbatch sizes affect input processing. Compare them on the same
uncached document; larger values are not automatically faster.

Embedded MTP uses Qwen's built-in draft predictor to suggest tokens for the
main model to verify. DFlash uses a separate draft model and needs additional
memory. Compare correctly completed tasks with the same sampling settings
and output budgets. A high rate of accepted draft tokens alone does not
establish a shorter task time.

## What the cache comparison found

Measured 14–15 September 2026 on an M5 with 32 GiB, two matched conversation
blocks on b10964 compared 14 checkpoints and 2 GiB of extra cache RAM with
16 checkpoints and no extra cache RAM. Both omitted earlier reasoning.
Returning to a conversation took
21–24 seconds with the extra cache, versus 103–110 seconds without it.
Answers and processing after earlier edits were preserved.

This benefit depended on the history fitting in the cache. At 26,624 tokens,
returning after switching threads still required full reprocessing. The
comparison did not measure time saved on a complete job.

The [model card](README.md) summarizes context and memory measurements.
The [llama.cpp reference](https://github.com/ggml-org/llama.cpp/blob/b29c606e28a01b1bc8c1351026a0fa6e616bf6c4/tools/server/README.md)
documents the exact options for the tested version.
