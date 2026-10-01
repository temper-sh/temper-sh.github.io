# Qwen3.8 27B for coding with Splash

We recommend **Qwen3.8 27B with Splash** for local coding on the tested M5 with
32 GiB. It produced useful first patches much sooner
than llama.cpp on two Flask tasks. Review and repairs remain part of the work.
This assessment is based on measurements and direct code review from
25–26 September 2026.

For installation, choose the Qwen/Splash preset in
[Temper](https://github.com/temper-sh/temper#get-started). Temper installs the
packaged engine and prepares the weights and template. That preset requires
Apple M3 or newer and macOS 26.4 or newer.

The measurements here used the earlier source build identified below. They
remain evidence for that exact setup, not a timing or quality test of the
current packaged release. Separate llama.cpp document and capacity results
are summarized on the [model card](../README.md).

## What we measured

| Component | Tested choice |
|---|---|
| Target weights | [Unsloth Qwen3.8 27B UD-Q4_K_XL GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF/tree/4ca720788d1e01f1bff70c033e0d0028fd02e502), Dynamic 3.0 |
| Engine | [Splash source fdc11c5](https://github.com/incoai/splash/tree/fdc11c5965313d7c0648c8be9b02ea385727fdd7) |
| Chat template | [Froggeric v22.5](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates/blob/855bffc49448e299789730ff92c9b8d834d6cc14/chat_template.jinja), supplied through the Python server's tokenizer override |
| Draft model | [Qwen3.8 27B DFlash2](https://huggingface.co/incoai/Qwen3.8-27B-DFlash2/tree/dedf8df68adfb1afeaf7b7480c0a0243108177b4), prepared by Splash |
| Request settings | Medium reasoning, previous thinking omitted; temperature 1, top-p 0.95, top-k 20 |
| Runtime settings | Language only, int8 conversation cache, 24G Metal allocation limit, 118,000-token total ceiling |

The GGUF preserves Unsloth's mixture of quantization formats and precisions.
DFlash2 is a separate draft model that proposes tokens for the target to verify.
The comparison also tested Splash with a different Q4 weight package. It
produced a syntax error and an invalid patch submission in the corresponding
Frog tests. The engine build also changed, so those observations do not isolate
weight compression as the cause.

## Measured coding work

Apple M5, 32 GiB, macOS 26.6.1. Each row is one first attempt, with seed 17 and
no repair feedback. Both engines received identical rendered prompts and token
IDs, the same GGUF target and Frog template. llama.cpp b11157 used embedded MTP
with three draft tokens and a 120,000-token ceiling; Splash used DFlash2 and
118,000. These are comparisons of the complete configurations.

| Task | Input tokens | llama.cpp time to submission | Splash time to submission | Splash output tokens |
|---|---:|---:|---:|---:|
| Fix async streaming | 16,078 | 102m 21s | 40m 27s | 37,455 |
| Extend template decorators | 23,398 | 58m 23s | 19m 21s | 21,902 |

Combined request time fell by **62.8%**. Timings include input processing and
reasoning, and exclude startup and subsequent review. No repair session was
timed, so this is a first-submission improvement rather than a measured reduction
in time to accepted code.

Both configurations passed the original and independent behavioral checks.
Both bug submissions failed two of their own tests. Splash's failing assertions
concerned when a streamed exception is raised and an inherited double-close
behavior. A separate review probe found a more consequential issue: its patch
suppressed a context-cleanup error outside the intended streaming fix. The
llama.cpp patch preserved that error guard and used a narrower mechanism.

Splash's feature passed all 175 checks, including 26 authored tests. Review
found an unnecessary second naming argument across six decorator APIs. Static
typing was not checked. These are useful proposals requiring review; passing
tests alone did not settle patch quality. The review knew candidate identities
and timings, and two tasks do not establish general quality parity.

Splash reported peak native allocations of 20.3–20.5 GiB, with no swap growth
or resource stop. This metric includes allocations that process RSS misses;
it is not minimum machine RAM. Smaller machines were not tested. The largest
observed sequence was 53,533 tokens, including reasoning and output. The 118k
allocation does not qualify 118k input, multi-turn coding sessions or vision.

[Model card](../README.md) · [Choose a model](../../../guides/choose-a-local-model.md).
