# Tools and resources

Software used in the experiments and setups documented here. Each entry
describes its role and links to setup details or test results.

## Models

- [General models](models/README.md) — Assessed models grouped by estimated
  memory fit, with links to capabilities, limitations and measurements.
- [Specialist models](models/specialists/README.md) — Models intended for a
  particular kind of work, grouped by task.
- [Choosing a local model](guides/choose-a-local-model.md) — Candidates by
  task and the main tradeoffs.

## Running models

- [llama.cpp](https://github.com/ggml-org/llama.cpp) — Runs GGUF model files.
  Its `llama-server` provides an HTTP interface for local applications.
  Model cards identify the engine build used in each measurement.
- [llama-swap](https://github.com/mostlygeek/llama-swap) — Routes requests to
  local model servers and can load or unload models as needed.
- [Splash](https://github.com/incoai/splash) — An Apple Silicon engine using a
  separate draft model to accelerate generation. Recommended for coding with
  the [tested Qwen3.8 27B GGUF and Frog configuration](models/qwen3.8-27b/layouts/splash-ud-q4-k-xl.md)
  on M5/32 GiB. Temper offers a ready-to-install Splash preset for M3 or newer
  on macOS 26.4+. The comparison identifies the older source build measured.
- [Temper](https://github.com/temper-sh/temper) — Installs and checks a selected
  configuration of model files, engine and settings. The
  [preset guide](catalog/README.md)
  explains the available choices.

## Working with models

- [Pi](https://github.com/earendil-works/pi/tree/main/packages/coding-agent) — A
  terminal coding agent with tools, sessions and extensions. The
  [Pi setup guide](guides/pi-setup.md) covers web search and remote access.

## Web search

- [Pi Web Access](https://github.com/nicobailon/pi-web-access) — Adds web search
  and page fetching to Pi. Selected after a
  [review of alternatives and local integration trials](investigations/pi-web-search.md).
  Queries go to the configured external search provider, including when
  Pi's main model runs locally.

## Remote access

- [Un Bien](https://github.com/georgeharker/un-bien) — Connects a phone to Pi
  sessions through a relay. Used by Temper's maintainer; the
  [Pi setup guide](guides/pi-setup.md#remote-access-with-un-bien) explains
  the app, extension and relay.

## Running experiments

- [Field Kit](https://github.com/temper-sh/field-kit) — Runs defined experiments
  and records the machine, configuration and results. Reports stay local
  until the participant chooses to share them.

Tests of individual components do not establish how well every combination
works. The linked guides and investigations describe the setups actually used.

[Guides](guides/README.md) · [Investigations](investigations/README.md) ·
[Back to Temper](README.md).
