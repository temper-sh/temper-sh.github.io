# Making sense of local AI

A local AI setup needs to do useful work and leave enough memory for the rest
of your machine. This guide explains why the model, software and settings all
matter.

[Temper](https://github.com/temper-sh/temper#get-started) chooses those parts
together in presets. You can start there and use this guide when you want to
understand a tradeoff or read a model's results.

## What makes a working setup

Drafting a message from a PDF involves several parts: extracting readable
text, writing from the right facts and checking the result.

| Part | What it does |
|---|---|
| Model | Produces answers or tool calls using what it learned during training. |
| Engine | Runs the model on your hardware; Temper selects a suitable engine, such as llama.cpp, for each preset. |
| Router | Sends an application's requests to the selected model. Temper uses llama-swap, which also starts and stops model servers. |
| App or agent | Provides the interface for conversations, files and tools. |
| Supporting tools | Handle search, extraction, editing and checks. |
| Machine | Supplies memory, compute and storage for the whole setup. |

When comparing results, look for the combination that was tested and the work
it completed.

A **preset** brings together weights chosen for quality, a suitable engine and
tuned settings. A **layout** groups presets for a use, such as writing or
coding, and chooses which model an application uses by default. A utility
layout can keep small helpers available alongside a cloud assistant.

## A model still needs something to run it

A downloadable model includes learned numbers called **parameters**, plus
configuration and tokenizer files. The learned numbers are its **weights**.
An **inference engine** loads them and performs the computation; a chat app
or coding agent communicates with that engine.

A chat template and runtime settings determine how messages, tools, context
and answers are handled. Different files, engines, templates or settings can
therefore produce different behaviour from the same model.

## What `27B` tells you

`B` means **billion parameters**. A `27B` model has roughly 27 billion learned
values. Parameter count gives a rough indication of size and resource use.
Download size also depends on compression, while task quality depends on
training, architecture, tools and the work itself.

### Dense versus MoE

A **dense** model uses essentially the same full set of main layers for every
token it processes.

A **Mixture of Experts**, or **MoE**, model routes each token through selected
expert blocks. In a name such as `30B-A3B`, the first number is the total
parameter count and `A3B` means roughly 3 billion are active for each token.
All experts still need to be stored. The active count helps explain how much
work each token needs, but use the full model files and measurements from a
similar machine to judge memory requirements.

## Quantization: trading precision for fit

**Quantization** stores model weights with less precision, reducing their size.
A quantized model build is often called a **quant**.

Labels such as `Q8`, `Q6`, `Q5`, `Q4` and `Q3` indicate compression
families. Lower numbers usually mean smaller files, with more opportunity to
affect accuracy, instruction following or tool use. Two nominally 4-bit builds
can allocate precision differently, and some parts may remain at higher
precision. A smaller build is not necessarily faster on every engine.

`UD` means **Unsloth Dynamic**. Its
[quantization guide](https://unsloth.ai/docs/basics/dynamic-3.0-ggufs) describes
model-specific allocations of precision. `Q4_K_M` is another mixed recipe
from the GGUF/llama.cpp ecosystem; the
[llama.cpp guide](https://github.com/ggml-org/llama.cpp/blob/master/tools/quantize/README.md)
explains how the stored number formats can differ within one model.

Compare actual file sizes and task results when choosing a build. The engine,
conversation cache and working buffers need additional memory.

## Reading one model name

| Part of `unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_XL` | Meaning |
|---|---|
| `unsloth/` | The account that packaged the files, which may differ from the original model developer. |
| `Qwen3.8` | Model family and generation. |
| `27B` | Approximate parameter count. |
| `GGUF` | Model-file format. |
| `UD-Q4_K_XL` | The publisher's quantization recipe and tier. |

Publishers also add dates, fine-tune names and engine-specific markers.
Check the exact model description for unfamiliar suffixes. Runtime memory,
usable context, speed and tool compatibility need separate measurements.

## Files, engines and apps

| Term | Meaning |
|---|---|
| **Model artifact** | The exact downloadable weights and associated files. |
| **GGUF** | A file format containing model data and metadata for GGML-based engines. It supports several quantization types. |
| **llama.cpp** | An inference engine and toolset that runs GGUF models across many kinds of hardware. |
| **MLX** | Apple's numerical-computing framework for Apple Silicon, used by local model runners and packages. |
| **App or agent** | The interface that sends conversations, tools and tasks to the engine. |

The [GGUF specification](https://github.com/ggml-org/ggml/blob/master/docs/gguf.md)
describes the file format. Apple's
[MLX documentation](https://ml-explore.github.io/mlx/build/html/usage/unified_memory.html)
explains its use of memory shared by the CPU and GPU.

Engines differ in speed, memory use, supported features and hardware
compatibility. Results from one engine need checking before applying them to
another.

## What actually has to fit

Allow memory for the model weights, the engine's working space, the
conversation and any other loaded tools or models.

On a machine with a discrete GPU, system RAM and GPU memory are separate
pools. Apple Silicon shares one pool between CPU and GPU, including the
operating system and other applications. A 32 GB machine has less than 32 GB
available for model weights.

Test a complete task: memory use can grow after the model has loaded.

### Context and the KV cache

The **context window** holds the material used in a request: instructions,
conversation history, documents, tool results and the answer being generated.
Its size is measured in **tokens**, which are pieces of text.

The engine keeps a **KV cache**, working memory for that context. It usually
grows with the context and may be quantized separately from the model weights.

| Context figure | What it tells you |
|---|---|
| Advertised by the model | The publisher's stated limit. |
| Accepted by the engine | What the software permits you to configure. |
| Tested on your kind of machine | What completed the measured workload under the stated memory and output limits. |

Start from a tested configuration. Leave room for the answer and follow-up
turns as well as the initial document.

## Speed is more than tokens per second

| Measure | What it includes |
|---|---|
| **Prefill speed** | Processing the prompt and existing context. |
| **Generation speed** | Producing new tokens. |
| **Time to done** | Reaching an acceptable result, including loading, tools, retries, checks and repairs. |

Prefill matters when reading large documents or repositories. Generation
speed affects how long the answer takes to arrive. For practical comparisons,
record whether the task finished correctly and how much time checking and
correction added.

## Small settings can change the whole setup

A **chat template** converts messages and tool calls into the token pattern
the model expects. An incompatible template can break instruction following
or tool use.

A **thinking mode** may improve reasoning at the cost of more tokens and time.
**Speculative decoding** uses a draft model or the target model's own
prediction heads to suggest upcoming tokens for the target to verify.
Generation can become faster when enough suggestions are accepted; the
technique does not improve the model's reasoning ability.

Changes to the template, mode, engine, context or quantization can affect the
result. Keep those settings with any measurements you compare.

The [model-choice guide](choose-a-local-model.md) connects these tradeoffs to
useful tasks and assessed models. Technical sources were checked on
**1 September 2026**; that source review did not retest any model setup.

[All guides](README.md) · [Back to Temper](../README.md).
