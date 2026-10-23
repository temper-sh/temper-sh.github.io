# Tools and resources

A directory of local-AI software, integrations and learning material, grouped
by role. Entries explain what a project does. Linked setup notes and experiments
identify what we have used or tested; inclusion alone does not establish how
well a tool or combination works.

## Learn the pieces

- [Making sense of local AI](guides/basics.md) — Our beginner's guide to models,
  weights, engines, compression, memory and context.
- [Models, tasks and hardware](guides/choose-a-local-model.md) — Questions to
  consider when putting together a setup.
- [How we assess local AI](guides/our-approach.md) — The scope and limits of our
  experiments.
- [Technical references](guides/technical-reference.md) — Configuration details,
  evidence and a reading route for setup agents.

## Find models and prepared configurations

- [Temper presets](catalog/README.md) — Prepared weights/engine/settings
  combinations, with reasons for the choices and links to observations.
- [General models](models/README.md) — Model records grouped by estimated
  memory fit, including configurations outside the installable catalog.
- [Specialist models](models/specialists/README.md) — Extraction, reranking and
  translation models, with their input/output interfaces and resource costs.

## Run and manage models

- [Temper](https://github.com/temper-sh/temper) — Prepares selected model files,
  engines and settings; manages layouts and explicit updates.
  [Explore its presets](catalog/README.md).
- [llama.cpp](https://github.com/ggml-org/llama.cpp) — Runs GGUF model files.
  Its `llama-server` provides an HTTP interface for applications.
- [Splash](https://github.com/incoai/splash) — An Apple Silicon engine with
  draft-model acceleration. [Qwen / Splash](models/qwen3.8-27b/layouts/splash-ud-q4-k-xl.md)
  documents one Temper preset and its observed behavior.
- [llama-swap](https://github.com/mostlygeek/llama-swap) — Routes requests to
  local model servers and loads or unloads models as needed.

## Work with models

- [Pi](https://github.com/earendil-works/pi/tree/main/packages/coding-agent) — A
  terminal coding agent with tools, sessions and extensions.
  [Our setup notes](guides/pi-setup.md) cover web search and remote access.

## Add web search

- [Pi Web Access](https://github.com/nicobailon/pi-web-access) — Adds web search
  and page fetching to Pi. Queries go to an external search provider, including
  when the main model runs locally.
  [Integration trials and alternatives](investigations/pi-web-search.md).

## Access sessions remotely

- [Un Bien](https://github.com/georgeharker/un-bien) — Connects a phone to Pi
  sessions through a relay. [Setup notes](guides/pi-setup.md#remote-access-with-un-bien)
  describe the app, extension and relay used by Temper's maintainer.

## Run experiments

- [Field Kit](https://github.com/temper-sh/field-kit) — Runs defined experiments
  and records the machine, configuration and results. Reports stay local until
  the participant chooses to share them.
- [Investigations](investigations/README.md) — Our experiments on particular
  tools and techniques, with their methods and limitations.

[Guides](guides/README.md) · [Back to Temper](README.md).
