# Temper

Temper sets up and maintains local AI models on Apple Silicon Macs. A **preset**
combines particular model weights, an engine, settings and compatibility fixes.
Temper installs those parts and connects them to the applications you use.
Different presets can use different engines, according to the model and hardware.

Use a local model for conversation, writing or coding, or a specialist for
extraction, translation or search ranking. Once installed, local inference can
work offline and keep prompts on your Mac. Online tools and cloud connections
are separate choices.

**[Explore presets](catalog/README.md)** ·
**[Install and set up Temper](https://github.com/temper-sh/temper#get-started)**

## Explore the documentation

| What you need | Where to go |
|---|---|
| Understand the terminology | [Making sense of local AI](guides/basics.md): models, weights, engines, memory and context. |
| See the prepared configurations | [Presets](catalog/README.md): what each contains, why it exists and links to observations. |
| Work out what matters for your setup | [Models, tasks and hardware](guides/choose-a-local-model.md): questions to consider and examples to explore. |
| Connect applications and use the models | [Setup and usage guides](guides/README.md#setup-and-use), including Pi. |
| Discover software and learning resources | [Tools and resources](tools.md), grouped by their role. |
| Understand the evidence or configure with an agent | [Technical references](guides/technical-reference.md) and [investigations](investigations/README.md). |

## What Temper takes care of

Temper prepares the model files and serving software, applies the preset's
settings, and provides a local connection for your apps. The preset pages explain
the choices behind those configurations, including template fixes and adapters.

A **layout** groups the presets you want available. Models can load on demand
through llama-swap, so selecting several does not keep them all in memory.
Saved choices change through explicit updates.

Small models can be main assistants or helpers, including on larger machines
where you want to leave resources for other work. Most local measurements use
an M5 with 32 GiB; some cover an M5 Pro with 64 GiB. Fit on actual 8 or 16 GiB
machines remains unmeasured.

## Reading the observations

Our tests cover a limited set of tasks and configurations. The pages report
what happened, the time and memory involved, and what the checks missed.
They do not establish a general ranking or reliability across other work.
Publisher claims and local observations are identified separately.
[How we assess local AI](guides/our-approach.md) explains the method and limits.

Corrections and results from other machines are welcome through
[issues](https://github.com/temper-sh/temper-sh.github.io/issues).
[Field Kit](https://github.com/temper-sh/field-kit) runs experiments that record
the machine, configuration and results. Reports stay local until their owner
chooses to share them.
