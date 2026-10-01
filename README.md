# Temper

Find local AI models that can help with your work, with measured waiting times,
known weaknesses and guidance on what fits your Mac. These are Temper's model
assessments and practical guides.

The **[Temper tool](https://github.com/temper-sh/temper)** installs a
curated local stack: model weights, an engine selected for the model and
hardware, and tuned settings, with llama-swap managing the local servers.
[Install and set up Temper](https://github.com/temper-sh/temper#get-started).

Start with [choosing a local model](guides/choose-a-local-model.md). Each
assessment explains the tasks and configuration tested, the result, and how
much checking and correction it still needs.

## Contents

| Subject | Pages |
|---|---|
| Fundamentals | [Making sense of local AI](guides/basics.md) |
| Limited memory | [What has to fit](guides/basics.md#what-actually-has-to-fit) |
| Model selection | [Choosing a local model](guides/choose-a-local-model.md) |
| Local helpers for a cloud assistant | [Combining local and cloud models](guides/choose-a-local-model.md#use-a-local-model-beside-another-assistant) |
| Models | [Model catalog](models/README.md) |
| Software | [Tools and resources](catalog/README.md) |
| Pi, web search and remote access | [Pi setup](guides/pi-setup.md) |
| Tested claims and techniques | [Investigations](investigations/README.md) |

Most local measurements here come from an Apple M5 with 32 GiB of memory.
Fit on 8 or 16 GiB machines remains unmeasured. The model cards identify the
configuration and hardware behind each result.

## Using several models

A pipeline might use one model to extract facts, another to draft from them,
and a program to check required fields. Running the steps in sequence can
avoid keeping every model in memory at once. The extra loading, coordination
and checking all count towards the cost of finishing the task.

The [approach guide](guides/our-approach.md#improve-the-harness-as-well-as-the-model)
explains how tools, instructions and checks affect an assistant's work. The
[guide to combining models](guides/choose-a-local-model.md#use-a-local-model-beside-another-assistant)
also considers local helpers for a cloud assistant.

## Testing

Claims about model quality, speed and token savings are starting points for
experiments with specific tasks and configurations.

In the [hashline editing comparison](investigations/hashline-editing.md),
an alternative edit format reduced median output tokens by 20.8%, but
Qwen3.6 27B produced the correct file in 24 of 32 attempts, down from 30.
We kept Pi's built-in string replacement for that setup.

Pages summarize the method, measured conditions and limitations so you can
understand each conclusion here.

## Projects

| Project | What it does |
|---|---|
| [Temper](https://github.com/temper-sh/temper) | Installs models and serving software, applies configuration and checks the installed files. |
| [Field Kit](https://github.com/temper-sh/field-kit) | Runs defined experiments and records the machine, configuration and results for review. |

Each project documents its requirements and release status. A model assessment
here does not mean that every tested configuration is available through Temper.

## Corrections and contributions

Corrections and results from other machines are welcome through
[issues](https://github.com/temper-sh/temper-sh.github.io/issues). For a model
result, include the machine, model build, serving software, settings and task.

[Field Kit](https://github.com/temper-sh/field-kit) provides experiments that
record those conditions. Results stay on the participant's machine until they
choose to share them.
